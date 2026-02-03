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
  - "[[1_Калькулятор удобрений_ред]]"
  - "[[Алгоритм работы с раздельными файлами]]"
Категория:
Дата:
---
При разработке используй реальную базу данных npk-database.php.

ИСПОЛЬЗУЙ В КОДЕ:
1. NPK_Database::get_fertilizers() - массив удобрений
2. NPK_Database::get_concentration_profiles() - профили грунтов
3. NPK_Database::get_constants() - пороговые значения

State определяется ТОЛЬКО в Calculation_Engine.
Data_Adapter НЕ вызывает Calculation_Engine.



# ФАЙЛ: `includes/class-data-adapter.php`
# СЛОЙ: Адаптация
# НАЗНАЧЕНИЕ: Подготовка и адаптация данных из `NPK_Database` для передачи в JavaScript. Исключительно для инициализации UI.


❗ Data_Adapter НЕ определяет, НЕ вычисляет и НЕ интерпретирует state.
❗ Любые диапазоны состояний передаются в JS исключительно как справочные данные.


При разработке используй реальную базу данных npk-database.php.

ИСПОЛЬЗУЙ В КОДЕ:
1. NPK_Database::get_fertilizers() - массив удобрений
2. NPK_Database::get_concentration_profiles() - профили грунтов
3. NPK_Database::get_constants() - пороговые значения

State определяется ТОЛЬКО в Calculation_Engine.
Data_Adapter НЕ вызывает Calculation_Engine.


## 🎯 ОСНОВНАЯ ЦЕЛЬ ЭТОГО ФАЙЛА
Получить "сырые" данные из `NPK_Database`, обработать их для удобства использования в JavaScript (сортировка, форматирование, добавление вычисляемых полей), подготовить и вернуть структурированный объект для `wp_localize_script()`. **Не участвует в цепочке расчётов, AJAX или определении state.**

## 📊 МЕСТО В АРХИТЕКТУРЕ
- **Находится в слое:** Адаптация (подготовка данных для представления)
- **Может вызывать:** Только статические методы класса `NPK_Database` (`::get_fertilizers()`, `::get_concentration_profiles()`, `::get_constants()`).
- **Не может вызывать:** `Calculation_Engine`, `AJAX_Handler`, `Shortcode`. Не может выполнять бизнес-расчёты.
- **Вызывается из:** `class-shortcode.php` или `concentration-module.php` для получения данных, которые будут переданы в JS через `wp_localize_script()`.

## ✅ РАЗРЕШЕНО (явный список)
1.  Чтение данных из `NPK_Database`.
2.  Сортировку удобрений по возрастанию `id` для отображения в `<select>`.
3.  Преобразование строк NPK ("16-16-16") в массивы `[16, 16, 16]` и вычисление суммы NPK (`npk_sum`).
4.  Приведение типов данных ТОЛЬКО на уровне формата (например, (float), (int)) без изменения смысловых значений.

5.  Добавление флагов (например, `is_default: true`) к профилю грунта по умолчанию ('wet').
6.  Формирование объекта со структурами, точно соответствующими ТЗ п.7.3.1 (`fertilizers`, `profiles`, `constants`, `defaults`).

## ❌ ЗАПРЕЩЕНО (явный список, с акцентом на прошлые ошибки)
1.  **НИКАКОГО участия в расчётах или AJAX-цепочке** (прошлая ошибка: чаты добавляли логику расчёта или передачи данных в `Calculation_Engine`).
2.  Определение `state` или любые операции, связанные с бизнес-логикой (прошлая ошибка: дублирование алгоритмов `Calculation_Engine`).
3.  Генерация HTML, CSS или JS-кода.
4.  Вызов каких-либо функций, кроме методов `NPK_Database`.
5.  Изменение исходных данных из БД (только преобразование формата).
6.  Предоставление данных куда-либо, кроме как для `wp_localize_script()`.
7. 7. Любая числовая валидация, коррекция, ограничение диапазонов или подмена значений, которые могут повлиять на расчёты.


## 📋 ОБЯЗАТЕЛЬНЫЕ МЕТОДЫ/ФУНКЦИИ
```php
<?php
/**
 * Data Adapter для подготовки данных БД к использованию в JavaScript UI.
 */
class NPK_Concentration_Data_Adapter {

    /**
     * Основной метод, возвращающий все данные, необходимые для инициализации интерфейса.
     * @return array Ассоциативный массив, готовый для передачи в wp_localize_script().
     */
    public static function get_ui_init_data(): array {
        // 1. Получить данные из NPK_Database.
        // 2. Обработать удобрения (сортировка, преобразование NPK, вычисление суммы).
        // 3. Обработать профили (добавить флаг is_default).
        // 4. Определить значения по умолчанию (удобрение с min id, профиль 'wet').
        // 5. Вернуть структуру согласно ТЗ.
    }

    /**
     * Обрабатывает массив удобрений из БД для использования в UI.
     * - Сортирует по возрастанию id.
     * - Преобразует npk строку в массив и вычисляет сумму.
     * - Валидирует solubility для отображения.
     * @param array $fertilizers Массив удобрений из NPK_Database::get_fertilizers().
     * @return array Обработанный массив удобрений.
     */
    protected static function adapt_fertilizers(array $fertilizers): array {
        // Сортировка usort по 'id'.
        // Для каждого удобрения:
        //   $fert['npk_array'] = array_map('intval', explode('-', $fert['npk']));
        //   $fert['npk_sum'] = array_sum($fert['npk_array']);
        //   $fert['solubility'] = self::validate_solubility_for_display($fert['solubility_total']);
        // Возврат массива.
    }

    /**
     * Валидация растворимости для отображения в UI. Отдельная от логики Calculation_Engine!
     * Гарантирует, что JS получит число от 1 до 100.
     * @param mixed $value Значение solubility_total из БД.
     * @return float Число от 1 до 100.
     */


    /**
     * Обрабатывает профили концентрации для UI.
     * - Добавляет флаг is_default: true для профиля с ключом 'wet'.
     * @param array $profiles Профили из NPK_Database::get_concentration_profiles().
     * @return array Обработанные профили.
     */
    protected static function adapt_profiles(array $profiles): array {
        foreach ($profiles as $key => &$profile) {
            $profile['is_default'] = ($key === 'wet');
        }
        return $profiles;
    }

    /**
     * Определяет начальные значения по умолчанию для UI.
     * @param array $adapted_fertilizers Обработанный массив удобрений (уже отсортирован).
     * @param array $adapted_profiles Обработанные профили грунтов.
     * @return array Массив ['fertilizer_id' => int, 'ground_type' => string, 'concentration' => float, 'water_volume' => float]
     */
    protected static function get_default_values(array $adapted_fertilizers, array $adapted_profiles): array {
        // fertilizer_id = первый элемент (с минимальным id) из $adapted_fertilizers.
        // ground_type = 'wet'.
        // concentration = 0.3
        // water_volume = 10.0
    }
}
```

## 🔗 ЗАВИСИМОСТИ И ДАННЫЕ
- **Получает данные из:** Только `NPK_Database::get_fertilizers()`, `::get_concentration_profiles()`, `::get_constants()`.
- **Передает данные в:** Вызывающий код (`Shortcode`), который передаст их в `wp_localize_script()` для использования в `calculator.js`.
-  **Константы:** Передает `constants` из `NPK_Database` в JS без интерпретации, анализа или использования в логике.


## ⚠️ КРИТИЧЕСКИЕ ТРЕБОВАНИЯ (нарушение = неприемка)
1.  **ИЗОЛЯЦИЯ ОТ БИЗНЕС-ЛОГИКИ:** Класс **НЕ ДОЛЖЕН** содержать или дублировать формулы расчёта массы или алгоритм определения `state` из `Calculation_Engine`. Его валидация — только для целей корректного отображения.
2.  **ТОЛЬКО ДЛЯ ИНИЦИАЛИЗАЦИИ UI:** Подготовленные данные используются **единожды** при загрузке страницы для рендеринга интерфейса. Они не используются для последующих расчётов.
3.  **НЕИЗМЕННОСТЬ ИСХОДНЫХ ДАННЫХ:** Исходные массивы из БД не должны модифицироваться, только преобразовываться в новый формат.
4.  **ЧЕТКАЯ СТРУКТУРА ВОЗВРАЩАЕМЫХ ДАННЫХ:** Возвращаемый массив **ОБЯЗАН** в точности соответствовать структуре, описанной в ТЗ п.7.3.1 (включая поля `npk_array`, `npk_sum`, `is_default`).

## 📝 ПРИМЕРЫ/ШАБЛОНЫ (если применимо)
```php
// Пример ожидаемого вывода get_ui_init_data():
return [
    'constants' => NPK_Database::get_constants(), // без изменений
    'fertilizers' => [ // отсортированный и обработанный массив
        [
            'id' => 1,
            'name' => 'Азофоска (Нитроаммофоска)',
            'npk_array' => [16, 16, 16],
            'npk_sum' => 48,
            'solubility' => 95.0,
            'type' => 'complex',
            'notes' => 'Применяется для...'
        ],
        // ...
    ],
    'profiles' => [
        'dry' => ['name' => 'Сухой грунт', 'states' => [...], 'is_default' => false],
        'medium' => ['name' => 'Слабо влажный', 'states' => [...], 'is_default' => false],
        'wet' => ['name' => 'Влажный грунт', 'states' => [...], 'is_default' => true],
    ],
    'defaults' => [
        'fertilizer_id' => 1,
        'ground_type' => 'wet',
        'concentration' => 0.3,
        'water_volume' => 10.0
    ]
];

---
