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

# ФАЙЛ: `concentration-module.php`
# СЛОЙ: Ядро модуля (Инициализация)
# НАЗНАЧЕНИЕ: Главный файл модуля, точка входа. Отвечает за инициализацию всех компонентов модуля, проверку зависимостей и предоставление API для интеграции с основным плагином.

## 🎯 ОСНОВНАЯ ЦЕЛЬ ЭТОГО ФАЙЛА
Служит **единой точкой входа и управления** для всего модуля концентрации. Его задача — корректно загрузить и инициализировать все внутренние классы (`Shortcode`, `AJAX_Handler`, `Data_Adapter`, `Calculation_Engine`), убедившись в наличии всех зависимостей (файл БД), и безопасно зарегистрировать функциональность модуля в WordPress. Не содержит бизнес-логики.

## 📊 МЕСТО В АРХИТЕКТУРЕ
- **Находится в слое:** Инициализация / Управление модулем
- **Может вызывать:** Методы инициализации всех внутренних классов модуля (`NPK_Concentration_Shortcode::init()`, `NPK_Concentration_AJAX_Handler::register_ajax_handlers()`), проверять существование файлов и классов (например, `NPK_Database`).
- **Не может вызывать:** Прямые методы бизнес-логики (например, `Calculation_Engine::calculate()`) или генерировать вывод.
- **Вызывается из:** Главного файла плагина `npk-calculators.php` (или может быть запущен как отдельный плагин). Вызов происходит через хук `plugins_loaded` или напрямую.

## ✅ РАЗРЕШЕНО (явный список)
1.  Проверять наличие критического файла `shared/config/npk-database.php` и класса `NPK_Database`.
2.  Определять константы модуля (пути, версию, префиксы).
3.  Подключать файлы классов модуля (`includes/*.php`) с проверкой на существование.
4.  Вызывать статические методы `init()` или `register()` у каждого внутреннего класса для их регистрации в WordPress.
5.  Предоставлять фильтры/хуки для основной темы или плагина для контроля загрузки модуля.
6.  Регистрировать переводы (локализацию) модуля.
7.  Содержать информацию о модуле для WordPress (заголовок, версию, описание), если он работает как отдельный плагин.

## ❌ ЗАПРЕЩЕНО (явный список, с акцентом на прошлые ошибки)
1.  **СОДЕРЖАТЬ БИЗНЕС-ЛОГИКУ** (прошлая ошибка: чаты помещали формулы или расчёты прямо в этот файл).
2.  **ДУБЛИРОВАТЬ ФУНКЦИОНАЛЬНОСТЬ** других классов (например, регистрировать шорткод или AJAX напрямую, минуя соответствующие классы).
3.  **ПРЯМО ОБРАЩАТЬСЯ К БД** или обрабатывать данные для UI (это работа `Data_Adapter` и `Calculation_Engine`).
4.  Генерировать HTML-вывод.
5.  Быть точкой входа для AJAX-запросов (это делает `AJAX_Handler` через `wp_ajax_*`).

## 📋 ОБЯЗАТЕЛЬНЫЕ МЕТОДЫ/ФУНКЦИИ
```php
<?php
static $initialized = false;
if ($initialized) {
    return;
}
$initialized = true;


// Безопасность: предотвращаем прямое выполнение файла.
if (!defined('ABSPATH')) {
    exit;
}

/**
 * Основной класс-инициализатор модуля концентрации.
 */
class NPK_Concentration_Module {

    /**
     * Версия модуля.
     */
    const VERSION = '1.0.0';

    /**
     * Инициализирует модуль. Главная точка входа.
     * @return void
     */
    public static function init(): void {
        // 1. Проверить зависимости (файл БД).
        if (!self::check_dependencies()) {
            add_action('admin_notices', [__CLASS__, 'display_dependency_error']);
            return; // Не инициализируем модуль дальше.
        }

        // 2. Загрузить файлы классов модуля.
        self::load_includes();

        // 3. Инициализировать компоненты модуля.
        self::init_components();
    }

    /**
     * Проверяет наличие всех необходимых для работы модуля зависимостей.
     * @return bool True если все зависимости удовлетворены.
     */
    protected static function check_dependencies(): bool {
        // Критически важный файл базы данных.
        $db_file_path = plugin_dir_path(__FILE__) . '../shared/config/npk-database.php';
        if (!file_exists($db_file_path)) {
            return false;
        }
        // Подключаем файл, чтобы проверить наличие класса.
        require_once $db_file_path;
        return class_exists('NPK_Database');
    }

    /**
     * Выводит административное уведомление об ошибке зависимостей.
     * @return void
     */
    public static function display_dependency_error(): void {
        ?>
        <div class="notice notice-error">
            <p><strong>Ошибка модуля "Калькулятор концентрации":</strong> Не найден основной файл базы данных (npk-database.php). Модуль отключен.</p>
        </div>
        <?php
    }

    /**
     * Подключает все необходимые файлы классов модуля из папки includes/.
     * @return void
     */
    protected static function load_includes(): void {
        $includes_dir = __DIR__ . '/includes/';
        $files_to_load = [
            'class-calculation-engine.php',
            'class-data-adapter.php',
            'class-ajax-handler.php',
            'class-shortcode.php'
        ];

        foreach ($files_to_load as $file) {
            $file_path = $includes_dir . $file;
            if (file_exists($file_path)) {
                require_once $file_path;
            } else {
                // Можно залогировать ошибку, но для простоты выходим.
                error_log(sprintf('NPK Concentration Module: missing file %s', $file));
return;
;
            }
        }
    }

    /**
     * Инициализирует все компоненты модуля, вызывая их методы регистрации.
     * @return void
     */
    protected static function init_components(): void {
        // Порядок может быть важен: например, AJAX-обработчики должны быть зарегистрированы до init.
        NPK_Concentration_AJAX_Handler::register_ajax_handlers();
        NPK_Concentration_Shortcode::init();
        // Calculation_Engine и Data_Adapter не требуют явной инициализации, они используются по требованию.
    }
}

// Запуск модуля на хуке plugins_loaded, чтобы быть уверенным в загрузке WordPress.
add_action('plugins_loaded', ['NPK_Concentration_Module', 'init']);

// Если модуль запущен как отдельный плагин, регистрируем хук активации/деактивации.
register_activation_hook(__FILE__, ['NPK_Concentration_Module', 'on_activation']);
register_deactivation_hook(__FILE__, ['NPK_Concentration_Module', 'on_deactivation']);

class NPK_Concentration_Module {
    /**
     * Хук активации модуля.
     */
    public static function on_activation(): void {
        flush_rewrite_rules();
    }

    /**
     * Хук деактивации модуля.
     */
    public static function on_deactivation(): void {
        flush_rewrite_rules();
    }


```

## 🔗 ЗАВИСИМОСТИ И ДАННЫЕ
- **Получает данные из:** Проверяет существование файла `../shared/config/npk-database.php` и класса `NPK_Database`.
- **Передает данные в:** Не передает данные, только инициализирует классы.
- **Использует константы WordPress:** `ABSPATH`, `WPINC`. Определяет свои: `VERSION`.
- **Использует файлы:** Все файлы в папке `includes/`, а также `../shared/config/npk-database.php`.

## ⚠️ КРИТИЧЕСКИЕ ТРЕБОВАНИЯ (нарушение = неприемка)
1.  **БЕЗОПАСНАЯ ИНИЦИАЛИЗАЦИЯ:** Модуль **НЕ ДОЛЖЕН** работать, если не найден файл `npk-database.php` или класс `NPK_Database`. Должна выводиться понятная ошибка в админке и работа должна быть остановлена.
2.  **ЕДИНАЯ ТОЧКА ВХОДА:** Все регистрации шорткодов, AJ-обработчиков и скриптов должны запускаться **отсюда**, через вызов методов соответствующих классов. Не должно быть разрозненных вызовов `add_shortcode` или `add_action` в других файлах, кроме классов модуля.
3.  **НЕЗАВИСИМОСТЬ ОТ ПОРЯДКА ЗАГРУЗКИ:** Код должен корректно работать, будучи подключенным как из основного плагина, так и как отдельный плагин.
4.  **ОТСУТСТВИЕ ПОБОЧНОЙ ФУНКЦИОНАЛЬНОСТИ:** Файл должен выполнять **только** задачи инициализации и управления жизненным циклом модуля. Никаких "лишних" функций.

## 📝 ПРИМЕРЫ/ШАБЛОНЫ (если применимо)
```php
// Пример безопасной проверки зависимостей в check_dependencies():
protected static function check_dependencies(): bool {
    // Путь должен быть относительным от местоположения этого файла.
    $db_file_path = plugin_dir_path(__FILE__) . '../shared/config/npk-database.php';
    
    if (!file_exists($db_file_path)) {
        error_log('NPK Concentration Module: Missing database file at ' . $db_file_path);
        return false;
    }
    
    require_once $db_file_path;
    
    if (!class_exists('NPK_Database')) {
        error_log('NPK Concentration Module: NPK_Database class not found after inclusion.');
        return false;
    }
    
    // Дополнительно можно проверить наличие обязательных методов.
    return true;
}
```

---
