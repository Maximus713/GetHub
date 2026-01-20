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
База данных не требуется.
Работай только с данными, которые приходят из других классов.


# ФАЙЛ: `assets/css/calculator.css`
# СЛОЙ: Стили
# НАЗНАЧЕНИЕ: Стилизация всех элементов интерфейса калькулятора концентрации. Обеспечение адаптивного, визуально согласованного и удобного отображения согласно макету ТЗ.

## 🎯 ОСНОВНАЯ ЦЕЛЬ ЭТОГО ФАЙЛА
Определить **внешний вид и layout** HTML-элементов, созданных в `calculator-template.php`. Обеспечить отзывчивость, доступность, плавные переходы и соответствие дизайн-требованиям ТЗ (цветовые схемы, размеры, расположение). Не содержит логики, только стили.

## 📊 МЕСТО В АРХИТЕКТУРЕ
- **Находится в слое:** Представление (Стили)
- **Может вызывать:** CSS-переменные (custom properties), медиа-запросы, ключевые кадры анимаций.
- **Не может вызывать:** JavaScript, PHP, не может генерировать или изменять HTML-разметку.
- **Вызывается из:** Подключается через `wp_enqueue_style()` в `NPK_Concentration_Shortcode`. Применяется браузером автоматически к соответствующему DOM.

## ✅ РАЗРЕШЕНО (явный список)
1.  Задавать размеры, отступы, границы, фоны для всех элементов калькулятора.
2.  Определять цветовые схемы: базовые цвета (основной, фона, текста), цвета состояний (`state.color`) будут применяться динамически через JS, но можно задать fallback.
3.  Реализовывать адаптивность через медиа-запросы (`@media`) для мобильных устройств.
4.  Создавать плавные переходы (`transition`) и простые анимации (`@keyframes`) для интерактивных элементов (наведение, нажатие, изменение значений).
5.  Использовать CSS-переменные для централизации ключевых значений (цвета, размеры, отступы).
6.  Обеспечивать доступность: достаточный цветовой контраст, focus-стили для клавиатурной навигации.
7.  Стилизовать состояния элементов: `:hover`, `:focus`, `:disabled`, `.active`.

## ❌ ЗАПРЕЩЕНО (явный список, с акцентом на прошлые ошибки)
1.  **СОДЕРЖАТЬ БИЗНЕС-ЛОГИКУ** (прошлая ошибка: чаты использовали CSS для условного отображения на основе данных, например, `content: attr(data-state)` с комплексными правилами).
2.  **ГЕНЕРИРОВАТЬ ИЛИ ИЗМЕНЯТЬ HTML** через `::before`/`::after` для критического контента (например, вставлять эмодзи состояния).
3.  **ЖЁСТКО ПРОПИСЫВАТЬ ЦВЕТА СОСТОЯНИЙ** (например, `background: #28a745` для `.state-optimal`). Цвета должны приходить из `state.color` и применяться инлайн или через CSS-переменные, заданные JS. В CSS можно определить только общие правила.
4.  Использовать `!important` без крайней необходимости (предпочтительно вообще не использовать).
5.  Задавать фиксированные размеры, ломающие адаптивность.

## 📋 ОБЯЗАТЕЛЬНЫЕ СЕЛЕКТОРЫ И СВОЙСТВА
CSS должен обеспечивать стилизацию для всех элементов, определённых в `calculator-template.php`. Ключевые области:

```css
/* 1. Основные переменные и сброс */
.npk-calc {
    --npk-primary-color: #2c3e50;
    --npk-bg-color: #f8f9fa;
    --npk-border-color: #dee2e6;
    --npk-border-radius: 8px;
    --npk-spacing-unit: 1rem;
    /* ... */
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    box-sizing: border-box;
}

/* 2. Панель выбора грунта */
.npk-calc-ground-selector {
    display: flex;
    gap: var(--npk-spacing-unit);
    margin-bottom: var(--npk-spacing-unit);
}
.ground-type-btn {
    flex: 1;
    padding: 0.75rem;
    border: 2px solid var(--npk-border-color);
    border-radius: var(--npk-border-radius);
    background: white;
    cursor: pointer;
    transition: all 0.15s ease;
}
.ground-type-btn.active {
    border-color: var(--npk-primary-color);
    font-weight: bold;
}

/* 3. Блок выбора удобрения */
.npk-calc-fertilizer-selector {
    display: flex;
    gap: var(--npk-spacing-unit);
    margin-bottom: var(--npk-spacing-unit);
}
#npk-fertilizer-select {
    flex: 3;
    padding: 0.75rem;
    border: 2px solid var(--npk-border-color);
    border-radius: var(--npk-border-radius);
}
.npk-calc-npk-display {
    flex: 2;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    font-weight: bold;
    background: var(--npk-bg-color);
    border-radius: var(--npk-border-radius);
}

/* 4. Панель ввода параметров */
.npk-calc-input-panel {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--npk-spacing-unit);
    margin-bottom: var(--npk-spacing-unit);
}
@media (max-width: 768px) {
    .npk-calc-input-panel {
        grid-template-columns: 1fr;
    }
}
.npk-calc-input-group label {
    display: block;
    margin-bottom: 0.25rem;
    font-size: 0.9rem;
    color: #666;
}
.npk-calc-input-with-buttons {
    display: flex;
}
.npk-calc-input-with-buttons button {
    width: 2.5rem;
    border: 2px solid var(--npk-border-color);
    background: var(--npk-bg-color);
    cursor: pointer;
    font-size: 1.2rem;
    user-select: none;
}
.npk-calc-input-with-buttons button:active {
    background: #ddd;
}
.npk-calc-input-with-buttons input {
    flex: 1;
    text-align: center;
    border: 2px solid var(--npk-border-color);
    border-left: none;
    border-right: none;
    padding: 0.75rem;
    min-width: 0; /* Важно для flex */
}
/* Специфичные стили для поля "Концентрация NPK" (только чтение) */
#npk-npk-concentration {
    background-color: #f8f9fa;
    border-color: #e9ecef;
    color: #6c757d;
    cursor: not-allowed;
}

/* 5. Слайдер концентрации */
.npk-calc-slider-container {
    margin-bottom: var(--npk-spacing-unit);
}
#npk-slider-label {
    text-align: center;
    padding: 0.5rem 1rem;
    border-radius: var(--npk-border-radius) var(--npk-border-radius) 0 0;
    font-weight: bold;
    color: white; /* Цвет будет динамическим, нужен контраст */
    margin-bottom: 0.5rem;
}
.npk-calc-slider-track {
    position: relative;
    height: 4px;
    background: #e9ecef;
    border-radius: 2px;
    margin: 1rem 0;
}
.npk-calc-slider-fill {
    position: absolute;
    height: 100%;
    border-radius: 2px;
    width: 50%; /* Будет изменяться JS */
    top: 0;
    left: 0;
}
#npk-concentration-slider {
    position: absolute;
    width: 100%;
    height: 100%;
    opacity: 0;
    cursor: pointer;
    z-index: 2;
}
.npk-calc-slider-ticks {
    display: flex;
    justify-content: space-between;
    font-size: 0.8rem;
    color: #6c757d;
}

/* 6. Блок результатов */
.npk-calc-results {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--npk-spacing-unit);
    text-align: center;
}
.npk-calc-result-item {
    padding: 1rem;
    background: var(--npk-bg-color);
    border-radius: var(--npk-border-radius);
}
.npk-calc-result-label {
    font-size: 0.9rem;
    color: #666;
    margin-bottom: 0.5rem;
}
.npk-calc-result-value {
    font-size: 1.5rem;
    font-weight: bold;
}
.npk-calc-result-state {
    padding: 0.5rem;
    border-radius: var(--npk-border-radius);
    color: white; /* Динамический контрастный цвет через JS */
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    font-weight: bold;
}

/* 7. Утилиты и состояния */
.npk-calc-error-toast {
    position: fixed;
    bottom: 1rem;
    right: 1rem;
    padding: 1rem;
    background: #dc3545;
    color: white;
    border-radius: var(--npk-border-radius);
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    z-index: 1000;
}
```

## 🔗 ЗАВИСИМОСТИ И ДАННЫЕ
- **Получает данные из:** Не получает. Статический файл.
- **Передает данные в:** Браузер. Стили применяются к элементам на основе их классов и ID.
- **Использует константы:** Определённые в самом CSS через `:root` или `.npk-calc` переменные.

## ⚠️ КРИТИЧЕСКИЕ ТРЕБОВАНИЯ (нарушение = неприемка)
1.  **АДАПТИВНОСТЬ:** Калькулятор должен корректно отображаться на мобильных устройствах (ширина от 320px). Панель ввода и результаты должны перестраиваться в колонку.
2.  **НЕЗАВИСИМОСТЬ ОТ ЦВЕТОВ СОСТОЯНИЙ:** CSS не должен определять конкретные цвета для `optimal`, `danger` и т.д. Он должен задавать правила, как применять цвет (например, `background: var(--current-state-color)`), где значение переменной устанавливается JS.
3.  **ОТСУТСТВИЕ "МАГИИ":** Стили не должны полагаться на нестандартные свойства или хаки, которые могут сломаться в будущих версиях браузеров.
4.  **ПОЛНАЯ СТИЛИЗАЦИЯ:** Должны быть простилизованы **все** визуальные состояния элементов: нормальное, ховер, фокус, активное, disabled.

## 📝 ПРИМЕРЫ/ШАБЛОНЫ
```css
/* Пример правильного использования переменной для цвета состояния */
#npk-slider-label,
.npk-calc-slider-fill,
.npk-calc-result-state {
    /* JS будет устанавливать это свойство инлайн или менять переменную */
    background-color: var(--current-state-color, #6c757d); /* fallback */
}

/* Пример медиа-запроса для мобильных */
@media (max-width: 480px) {
    .npk-calc-results,
    .npk-calc-input-panel {
        grid-template-columns: 1fr;
    }
    .npk-calc-ground-selector {
        flex-direction: column;
    }
}

/* Пример focus-стилей для доступности */
#npk-concentration-input:focus,
#npk-fertilizer-select:focus,
.ground-type-btn:focus {
    outline: 3px solid rgba(44, 62, 80, 0.5); /* var(--npk-primary-color) с прозрачностью */
    outline-offset: 2px;
}
```

---
