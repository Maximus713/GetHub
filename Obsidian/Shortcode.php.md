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

# ФАЙЛ: `includes/class-shortcode.php`
# СЛОЙ: Представление (Контроллер/Обертка)
# НАЗНАЧЕНИЕ: Регистрация шорткода WordPress, подключение ресурсов (JS/CSS) и вывод HTML-контейнера-обёртки для калькулятора


## 🎯 ОСНОВНАЯ ЦЕЛЬ ЭТОГО ФАЙЛА
Предоставить простой способ вставки калькулятора на любую страницу WordPress через шорткод `[npk_concentration_calculator]`. Отвечает за "сборку" представления: регистрацию скриптов/стилей, получение данных для инициализации и вывод HTML-шаблона. Не содержит бизнес-логики и не генерирует разметку калькулятора.

## 📊 МЕСТО В АРХИТЕКТУРЕ
- **Находится в слое:** Представление (контроллер уровня модуля)
- **Может вызывать:** `NPK_Concentration_Data_Adapter::get_ui_init_data()`, функции WordPress для регистрации/очереди скриптов и стилей (`wp_register_script`, `wp_enqueue_script`, `wp_localize_script`), функцию для включения HTML-шаблона (`load_template` или `include`).
- **Не может вызывать:** `NPK_Database`, `Calculation_Engine`, `AJAX_Handler`. Не выполняет расчёты и не обрабатывает запросы.
- **Вызывается из:** Ядра WordPress при обработке шорткода. Также инициализируется из `concentration-module.php`.

## ✅ РАЗРЕШЕНО (явный список)
1.  Регистрировать шорткод WordPress.
2.  Регистрировать и ставить в очередь JavaScript (`calculator.js`) и CSS (`calculator.css`) файлы модуля с правильными зависимостями и версиями.
3.  Получать данные для инициализации UI через `Data_Adapter` и передавать их в JavaScript с помощью `wp_localize_script()`.
4.  Выводить основной HTML-контейнер (например, `<div id="npk-concentration-calculator">`) и подключать шаблон (`templates/calculator-template.php`).
5.  Обеспечивать уникальность идентификаторов и классов для избежания конфликтов с другими модулями.
6.  Добавлять необходимые data-атрибуты в контейнер для JS.

## ❌ ЗАПРЕЩЕНО (явный список, с акцентом на прошлые ошибки)
1.  **НИКАКОЙ бизнес-логики, расчётов или определений state** (прошлая ошибка: чаты вставляли формулы или условия прямо в шорткод).
2.  **ПРЯМОЕ ОБРАЩЕНИЕ К `NPK_Database`** (прошлая ошибка: дублирование функциональности `Data_Adapter`).
3.  Генерация сложной HTML-разметки калькулятора внутри класса (разметка должна быть в шаблоне `templates/calculator-template.php`).
4.  Регистрация AJAX-обработчиков (это делает `AJAX_Handler`).
5.  Модификация данных, полученных от `Data_Adapter`, перед передачей в JS (кроме оборачивания для `wp_localize_script`).
6.  Использование инлайн-стилей или скриптов для реализации функциональности калькулятора.
7. 7. Передача данных, кроме HTML-контейнера и data-атрибутов, напрямую в шаблон.


## 📋 ОБЯЗАТЕЛЬНЫЕ МЕТОДЫ/ФУНКЦИИ
```php
<?php
/**
 * Shortcode handler for the Concentration Calculator module.
 */
class NPK_Concentration_Shortcode {

    /**
     * Регистрирует шорткод и хуки для подключения ресурсов.
     * Вызывается из concentration-module.php.
     * @return void
     */
    public static function init(): void {
        add_shortcode('npk_concentration_calculator', [__CLASS__, 'render_shortcode']);
        // Ресурсы можно подключать по условию, если шорткод на странице
    
    }

    /**
     * Регистрирует скрипты и стили модуля (не ставит в очередь).
     * @return void
     */
    public static function register_assets(): void {
        wp_register_script(
            'npk-concentration-calculator-js',
            plugins_url('assets/js/calculator.js', dirname(__FILE__)),
            [], // Зависимости: возможно, []
            '1.0.0',
            true // В footer
        );

        wp_register_style(
            'npk-concentration-calculator-css',
            plugins_url('assets/css/calculator.css', dirname(__FILE__)),
            [],
            '1.0.0'
        );
    }

    /**
     * Обработчик шорткода [npk_concentration_calculator].
     * Подключает ресурсы, получает данные, выводит контейнер и шаблон.
     * @param array $atts Атрибуты шорткода (не используются в первой версии).
     * @param string $content Контент внутри шорткода (не используется).
     * @return string HTML-код калькулятора.
     */
    public static function render_shortcode($atts = [], $content = null): string {
        
       // 1. Зарегистрировать ресурсы (если ещё не зарегистрированы) и поставить в очередь.
if (!wp_script_is('npk-concentration-calculator-js', 'registered')) {
    self::register_assets();
}
self::enqueue_assets();


        // 2. Получить данные для инициализации JS от Data_Adapter.
        $init_data = NPK_Concentration_Data_Adapter::get_ui_init_data();

        // 3. Локализовать скрипт, передав данные.
        wp_localize_script(
            'npk-concentration-calculator-js',
            'NPK_Concentration_Data', // Имя объекта в JS
            $init_data
        );

        // 4. Начать буферизацию вывода, включить шаблон, вернуть HTML.
        ob_start();
     include plugin_dir_path(dirname(__FILE__)) . 'templates/calculator-template.php';

        return ob_get_clean();
    }

    /**
     * Ставит зарегистрированные скрипты и стили в очередь.
     * @return void
     */
    protected static function enqueue_assets(): void {
        wp_enqueue_script('npk-concentration-calculator-js');
        wp_enqueue_style('npk-concentration-calculator-css');
    }
}
```

## 🔗 ЗАВИСИМОСТИ И ДАННЫЕ
- **Получает данные из:** `NPK_Concentration_Data_Adapter::get_ui_init_data()`.
- **Передает данные в:** JavaScript через `wp_localize_script('npk-concentration-calculator-js', 'NPK_Concentration_Data', ...)`.
- **Использует константы/функции WordPress:** `plugins_url()`, `plugin_dir_path()`, `add_shortcode()`, `wp_register_script()`, `wp_enqueue_script()`, `wp_localize_script()`, `add_action()`.
- **Использует файлы:** `assets/js/calculator.js`, `assets/css/calculator.css`, `templates/calculator-template.php`.

## ⚠️ КРИТИЧЕСКИЕ ТРЕБОВАНИЯ (нарушение = неприемка)
1.  **ЧИСТАЯ ОБЁРТКА:** Класс является **только обёрткой** для подключения компонентов. Вся основная логика UI (взаимодействие, расчёт) должна быть в `calculator.js`, разметка — в шаблоне.
2.  **КОРРЕКТНАЯ ПЕРЕДАЧА ДАННЫХ:** Данные, полученные от `Data_Adapter`, должны быть переданы в JS **без изменений**. Класс не должен их фильтровать или дополнять.
3.  **ИДЕМПОТЕНТНОСТЬ:** Шорткод может быть использован на странице несколько раз, но ресурсы должны быть зарегистрированы и поставлены в очередь только один раз. Не должно быть дублирования скриптов или конфликтов идентификаторов.
4.  **ИЗОЛЯЦИЯ ОТ ЯДРА:** Класс не должен знать о существовании `Calculation_Engine` или `AJAX_Handler`. Его задача — только представление.
5. 5. Шорткод НЕ управляет логикой инициализации калькулятора — только подключает ресурсы и выводит контейнер.


## 📝 ПРИМЕРЫ/ШАБЛОНЫ (если применимо)
```php
// Пример минимального содержимого render_shortcode():
public static function render_shortcode($atts = [], $content = null): string {
    // Всегда проверяем, зарегистрированы ли активы, прежде чем ставить в очередь.
    if (!wp_script_is('npk-concentration-calculator-js', 'registered')) {
        self::register_assets();
    }
    self::enqueue_assets();

    $init_data = NPK_Concentration_Data_Adapter::get_ui_init_data();
    wp_localize_script('npk-concentration-calculator-js', 'NPK_Concentration_Data', $init_data);

    ob_start();
    ?>
    <div id="npk-concentration-calculator" class="npk-concentration-calc-wrapper" data-init='<?php echo esc_attr(wp_json_encode($init_data['defaults'])); ?>'>
        <!-- Контейнер для React/Vue или место, куда calculator.js встроит интерфейс -->
        <?php include plugin_dir_path(dirname(__FILE__)) . 'templates/calculator-template.php'; ?>
    </div>
    <?php
    return ob_get_clean();
}
```

---
