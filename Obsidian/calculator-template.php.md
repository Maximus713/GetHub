---
tags:
  - Ledgreen_ru
  - Калькулятор
  - Концентрация
  - AI
Похожее:
Сфера:
  - "[[Бизнес]]"
  - "[[IT]]"
  - "[[Web]]"
Источник:
Ссылки:
  - "[[Калькулятор удобрений]]"
  - "[[Алгоритм работы с раздельными файлами]]"
Категория:
Дата:
---

# ФАЙЛ: `templates/calculator-template.php`
# СЛОЙ: Представление (HTML-Разметка)
# НАЗНАЧЕНИЕ: Содержит чистую HTML-разметку интерфейса калькулятора, готовую для встраивания через шорткод. Определяет структуру DOM-элементов, их иерархию и начальные data-атрибуты.

## 🎯 ОСНОВНАЯ ЦЕЛЬ ЭТОГО ФАЙЛА
Предоставить **статическую HTML-структуру** для калькулятора, которая будет динамически наполняться и управляться JavaScript (`calculator.js`). Содержит все необходимые элементы с правильными `id`, `class` и `data-*` атрибутами для однозначной привязки JS-логики. Не содержит PHP-логики (кроме минимальных echo для данных по умолчанию) и не генерирует CSS/JS.

## 📊 МЕСТО В АРХИТЕКТУРЕ
- **Находится в слое:** Представление (Static Markup)
- **Может вызывать:** Минимальные PHP-функции для экранирования вывода (`esc_attr`, `esc_html`), если переданы данные из `Shortcode`. В идеале — вообще не содержит PHP.
- **Не может вызывать:** Классы модуля (`Calculation_Engine`, `Data_Adapter`), выполнять запросы к БД, содержать бизнес-логику.
- **Вызывается из:** `NPK_Concentration_Shortcode::render_shortcode()` через `include`. Включается в вывод шорткода.

## ✅ РАЗРЕШЕНО (явный список)
1.  Определять полную, семантически верную HTML-структуру калькулятора согласно макету ТЗ п.5.
2.  Задавать уникальные `id` и классы (`class`) для всех интерактивных элементов (поля ввода, кнопки, слайдер, блоки результатов).
3.  Использовать `data-*` атрибуты для хранения неизменяемых справочных данных (например, `data-step`, `data-min`, `data-max` для полей ввода).
4.  Включать placeholder-тексты, подписи (`<label>`), неинтерактивные элементы (пояснительные блоки).
5.  Использовать минимальный PHP для подстановки значений по умолчанию (например, `value="<?php echo esc_attr($defaults['concentration']); ?>"`), если они переданы в шаблон.
6.  Следовать принципам доступности (ARIA-атрибуты, правильная связь label-for).

## ❌ ЗАПРЕЩЕНО (явный список, с акцентом на прошлые ошибки)
1.  **СОДЕРЖАТЬ PHP-ЛОГИКУ** расчета, определения state или обработки данных (прошлая ошибка: чаты вставляли в шаблоны вызовы `Calculation_Engine` или условия).
2.  **ГЕНЕРИРОВАТЬ INLINE-СТИЛИ** или `<style>` блоки, кроме критических для предотвращения CLS (Cumulative Layout Shift).
3.  **ВКЛЮЧАТЬ INLINE-JAVASCRIPT** (`<script>`) для реализации функциональности (вся логика в `calculator.js`).
4.  **ОБРАЩАТЬСЯ К БАЗЕ ДАННЫХ** или глобальным переменным WordPress напрямую.
5.  **МЕНЯТЬ СТРУКТУРУ DOM** на лету с помощью PHP (все динамические изменения — задача JS).
6.  **ДУБЛИРОВАТЬ РАЗМЕТКУ**, которая может быть сгенерирована JS (например, опции для `<select>` — их должен заполнить JS из `NPK_Concentration_Data`).

## 📋 ОБЯЗАТЕЛЬНЫЕ ЭЛЕМЕНТЫ И АТРИБУТЫ
Шаблон должен содержать следующие элементы с точными `id` (примеры):

```html
<!-- Основной контейнер -->
<div id="npk-concentration-calculator" class="npk-calc" data-version="1.0">

    <!-- 1. Панель выбора грунта -->
    <div class="npk-calc-ground-selector">
        <button type="button" class="ground-type-btn" data-ground-type="dry">Сухой</button>
        <button type="button" class="ground-type-btn" data-ground-type="medium">Слабо влажный</button>
        <button type="button" class="ground-type-btn active" data-ground-type="wet">Влажный</button>
    </div>

    <!-- 2. Пояснительный текст (заглушка) -->
    <p class="npk-calc-hint">Определите влажность грунта перед поливом...</p>

    <!-- 3. Блок выбора удобрения -->
    <div class="npk-calc-fertilizer-selector">
        <select id="npk-fertilizer-select" class="npk-calc-select">
            <!-- Опции будут добавлены JS из NPK_Concentration_Data.fertilizers -->
            <option value="" disabled>Загрузка...</option>
        </select>
        <div class="npk-calc-npk-display">
            <span id="npk-current-npk">– – –</span>
        </div>
    </div>

    <!-- 4. Панель ввода параметров -->
    <div class="npk-calc-input-panel">

        <!-- Объём воды -->
        <div class="npk-calc-input-group">
            <label for="npk-water-volume-input">Объём воды, л</label>
            <div class="npk-calc-input-with-buttons">
                <button type="button" class="npk-calc-btn-decrement" data-target="water-volume">−</button>
                <input type="number" id="npk-water-volume-input" 
                       step="0.5" min="0.5" max="200" value="10.0">
                <button type="button" class="npk-calc-btn-increment" data-target="water-volume">+</button>
            </div>
        </div>

        <!-- Общая концентрация -->
        <div class="npk-calc-input-group">
            <label for="npk-concentration-input">Общая концентрация, %</label>
            <div class="npk-calc-input-with-buttons">
                <button type="button" class="npk-calc-btn-decrement" data-target="concentration">−</button>
                <input type="number" id="npk-concentration-input" 
                       step="0.01" min="0.05" max="1.5" value="0.30">
                <button type="button" class="npk-calc-btn-increment" data-target="concentration">+</button>
            </div>
        </div>

        <!-- Концентрация NPK (только чтение) -->
        <div class="npk-calc-input-group">
            <label for="npk-npk-concentration">Только NPK, %</label>
            <input type="text" id="npk-npk-concentration" readonly value="0.144">
        </div>

    </div>

    <!-- 5. Шкала (слайдер) концентрации -->
    <div class="npk-calc-slider-container">
        <!-- Метка состояния над слайдером -->
        <div id="npk-slider-label" class="npk-calc-slider-label">Оптимальная</div>
        
        <!-- Сам слайдер -->
        <div class="npk-calc-slider-track">
            <div id="npk-slider-fill" class="npk-calc-slider-fill"></div>
            <input type="range" id="npk-concentration-slider" 
                   min="0.05" max="1.5" step="0.01" value="0.30"
                   aria-labelledby="npk-slider-label">
        </div>
        
        <!-- Технические метки под слайдером -->
        <div class="npk-calc-slider-ticks">
            <span>0.05%</span>
            <span>0.25%</span>
            <span>0.50%</span>
            <span>0.75%</span>
            <span>1.00%</span>
            <span>1.25%</span>
            <span>1.50%</span>
        </div>
    </div>

    <!-- 6. Блок результатов -->
    <div class="npk-calc-results">
        <div class="npk-calc-result-item">
            <div class="npk-calc-result-label">Масса удобрения</div>
            <div id="npk-result-mass" class="npk-calc-result-value">– г</div>
        </div>
        <div class="npk-calc-result-item">
            <div class="npk-calc-result-label">Концентрация</div>
            <div id="npk-result-conc" class="npk-calc-result-value">– %</div>
        </div>
        <div class="npk-calc-result-item">
            <div class="npk-calc-result-label">Состояние раствора</div>
            <div id="npk-result-state" class="npk-calc-result-state">
                <span class="npk-calc-state-emoji">⏳</span>
                <span class="npk-calc-state-name">Расчёт...</span>
            </div>
        </div>
    </div>

    <!-- Скрытые/служебные элементы -->
    <div id="npk-calc-error-toast" class="npk-calc-error-toast" aria-live="polite" hidden></div>
</div>
```

## 🔗 ЗАВИСИМОСТИ И ДАННЫЕ
- **Получает данные из:** (Опционально) Переменной, переданной при `include` в `Shortcode` (например, `$defaults`). В основном полагается на JS, который заполнит данные.
- **Передает данные в:** DOM. JavaScript читает структуру и атрибуты элементов.
- **Использует константы:** Жёстко заданные в разметке `min`, `max`, `step` для полей ввода и слайдера (должны соответствовать ТЗ п.5 и п.6.3).

## ⚠️ КРИТИЧЕСКИЕ ТРЕБОВАНИЯ (нарушение = неприемка)
1.  **ЧИСТЫЙ HTML:** Файл должен быть, по сути, **валидным HTML-фрагментом**. Любая логика, кроме минимальной подстановки значений по умолчанию, недопустима.
2.  **ПОЛНАЯ СТРУКТУРА:** Должны присутствовать **все** элементы интерфейса, перечисленные в ТЗ п.5, в правильной иерархии и порядке.
3.  **УНИКАЛЬНЫЕ ID:** Все `id` должны быть уникальными и соответствовать тем, на которые ссылается `calculator.js` (см. его `cacheDomElements`). Не должно быть конфликтов с другими элементами на странице.
4.  **ОТСУТСТВИЕ СТИЛЕЙ И СКРИПТОВ:** Вся стилизация — в `calculator.css`, вся логика — в `calculator.js`. Шаблон — только "скелет".

## 📝 ПРИМЕРЫ/ШАБЛОНЫ
```html
<!-- Пример правильного использования PHP в шаблоне (минимум): -->
<input type="number" id="npk-concentration-input" 
       step="0.01" min="0.05" max="1.5" 
       value="<?php echo isset($init_data['defaults']['concentration']) ? esc_attr($init_data['defaults']['concentration']) : '0.30'; ?>">

<!-- Пример НЕПРАВИЛЬНОГО использования PHP (запрещено!): -->
<input type="number" id="npk-concentration-input" 
       value="<?php 
           // ЗАПРЕЩЕНО: Обращение к Calculation_Engine из шаблона
           $mass = NPK_Concentration_Calculation_Engine::calculate(...);
           echo $mass;
       ?>">
```

---

