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
Категория:
Дата:
---


# ФАЙЛ: `npk-calculators.php`
# СЛОЙ: Ядро плагина (Главный файл)
# НАЗНАЧЕНИЕ: Главный файл WordPress-плагина "NPK Calculators". Отвечает за инициализацию всей системы, управление загрузкой модулей и обеспечение их совместной работы через централизованный механизм загрузки.

## 🎯 ОСНОВНАЯ ЦЕЛЬ ЭТОГО ФАЙЛА
Служить **центральным диспетчером загрузки** для всего плагина NPK Calculators. Его задача — корректно загрузить общие ресурсы (базу данных), проверить системные требования, и **на стандартном хуке WordPress инициализировать все модули**, обеспечивая правильный порядок загрузки и предотвращение конфликтов.

## 📊 МЕСТО В АРХИТЕКТУРЕ
```
WordPress → plugins_loaded → npk-calculators.php → module-concentration/ → шорткод
                    ↓
              другие модули
```
- **Находится в слое:** Ядро плагина (менеджер загрузки модулей)
- **Может вызывать:** Проверки WordPress, подключение файла БД, методы инициализации модулей на хуке `plugins_loaded`.
- **Не может вызывать:** Классы модулей напрямую в конструкторе (только через хук).
- **Вызывается из:** WordPress на хуке `plugins_loaded`.

## ✅ РАЗРЕШЕНО (явный список)
1.  Содержать стандартный заголовок плагина WordPress.
2.  Проверять минимальные требования (версия PHP, WordPress) **до активации плагина**.
3.  Подключать общий файл базы данных (`shared/config/npk-database.php`) с проверкой его существования.
4.  Определять константы плагина (пути, версии, префиксы).
5.  Регистрировать хуки активации/деактивации/удаления плагина.
6.  **Регистрировать инициализацию модулей на хуке `plugins_loaded` с правильным приоритетом**.
7.  Предоставлять общие хуки (фильтры/действия) для интеграции модулей между собой и с внешним кодом.
8.  Реализовывать механизм загрузки модулей через централизованный метод, вызываемый на `plugins_loaded`.

## ❌ ЗАПРЕЩЕНО (явный список)
1.  **ИНИЦИАЛИЗИРОВАТЬ МОДУЛИ НЕПОСРЕДСТВЕННО В КОНСТРУКТОРЕ** (критическая ошибка архитектуры).
2.  Загружать модули до выполнения хука `plugins_loaded`.
3.  **СОДЕРЖАТЬ БИЗНЕС-ЛОГИКУ** конкретных калькуляторов.
4.  **ДУБЛИРОВАТЬ ФУНКЦИОНАЛЬНОСТЬ МОДУЛЕЙ**.
5.  Создавать множественные экземпляры классов модулей.

## 📋 ОБЯЗАТЕЛЬНЫЕ МЕТОДЫ/ФУНКЦИИ (ОБНОВЛЁННЫЕ)
```php
<?php
/**
 * Plugin Name: NPK Calculators
 * Plugin URI: https://ваш-сайт/
 * Description: Набор калькуляторов для агрономических расчётов: концентрация растворов, подбор удобрений и др.
 * Version: 1.0.0
 * Author: Ваша компания
 * License: GPL v2 or later
 * Text Domain: npk-calculators
 */

// Безопасность: предотвращаем прямое выполнение файла.
if (!defined('ABSPATH')) {
    exit;
}

/**
 * Основной класс плагина NPK Calculators.
 * Управляет загрузкой всех модулей на хуке plugins_loaded.
 */
class NPK_Calculators {

    const VERSION = '1.0.0';
    const MIN_WP_VERSION = '5.0';
    const MIN_PHP_VERSION = '7.4';
    
    private static $instance = null;
    
    /**
     * Получить экземпляр плагина.
     * @return NPK_Calculators
     */
    public static function get_instance() {
        if (is_null(self::$instance)) {
            self::$instance = new self();
        }
        return self::$instance;
    }
    
    /**
     * Конструктор. Регистрирует хуки, НЕ загружает модули.
     */
    private function __construct() {
        // Только базовая инициализация
        $this->define_constants();
        $this->register_core_hooks();
    }
    
    /**
     * Определяет константы плагина.
     */
    private function define_constants() {
        define('NPK_CALCULATORS_PATH', plugin_dir_path(__FILE__));
        define('NPK_CALCULATORS_URL', plugin_dir_url(__FILE__));
        define('NPK_CALCULATORS_VERSION', self::VERSION);
    }
    
    /**
     * Регистрирует основные хуки плагина.
     */
    private function register_core_hooks() {
        // Проверка требований при каждой загрузке
        add_action('plugins_loaded', [$this, 'check_requirements'], 5);
        
        // Загрузка базы данных
        add_action('plugins_loaded', [$this, 'include_database'], 10);
        
        // Инициализация модулей (основная задача)
        add_action('plugins_loaded', [$this, 'init_modules'], 20);
        
        // Переводы
        add_action('plugins_loaded', [$this, 'load_textdomain'], 15);
        
        // Хуки активации/деактивации
        register_activation_hook(__FILE__, [$this, 'on_activation']);
        register_deactivation_hook(__FILE__, [$this, 'on_deactivation']);
    }
    
    /**
     * Проверяет системные требования. Вызывается на plugins_loaded.
     */
    public function check_requirements() {
        $errors = [];
        
        if (version_compare(get_bloginfo('version'), self::MIN_WP_VERSION, '<')) {
            $errors[] = sprintf(
                __('Требуется WordPress %s или выше. Текущая версия: %s.', 'npk-calculators'),
                self::MIN_WP_VERSION,
                get_bloginfo('version')
            );
        }
        
        if (version_compare(PHP_VERSION, self::MIN_PHP_VERSION, '<')) {
            $errors[] = sprintf(
                __('Требуется PHP %s или выше. Текущая версия: %s.', 'npk-calculators'),
                self::MIN_PHP_VERSION,
                PHP_VERSION
            );
        }
        
        if (!empty($errors)) {
            add_action('admin_notices', function() use ($errors) {
                echo '<div class="notice notice-error"><p>';
                echo '<strong>' . __('NPK Calculators: Ошибка требований', 'npk-calculators') . '</strong><br>';
                echo implode('<br>', $errors);
                echo '</p></div>';
            });
            
            // Деактивируем плагин, если требования не выполнены
            if (is_admin() && current_user_can('activate_plugins')) {
                deactivate_plugins(plugin_basename(__FILE__));
            }
        }
    }
    
    /**
     * Подключает общую базу данных. Вызывается на plugins_loaded.
     */
    public function include_database() {
        $db_file = NPK_CALCULATORS_PATH . 'shared/config/npk-database.php';
        
        if (!file_exists($db_file)) {
            add_action('admin_notices', function() {
                echo '<div class="notice notice-error"><p>';
                _e('NPK Calculators: Критическая ошибка - файл базы данных не найден.', 'npk-calculators');
                echo '</p></div>';
            });
            return;
        }
        
        require_once $db_file;
        
        if (!class_exists('NPK_Database')) {
            add_action('admin_notices', function() {
                echo '<div class="notice notice-error"><p>';
                _e('NPK Calculators: Класс NPK_Database не найден.', 'npk-calculators');
                echo '</p></div>';
            });
        }
    }
    
    /**
     * Основной метод инициализации всех модулей. Вызывается на plugins_loaded.
     */
    public function init_modules() {
        // Если есть ошибки требований или БД - не загружаем модули
        if (!$this->are_requirements_met()) {
            return;
        }
        
        // 1. Модуль концентрации
        $this->load_module('concentration', 'module-concentration/concentration-module.php', 'NPK_Concentration_Module');
        
        // 2. Будущие модули будут добавлены здесь
        // $this->load_module('fertilizer-selector', 'module-fertilizer-selector/module.php', 'NPK_Fertilizer_Selector_Module');
    }
    
    /**
     * Загружает отдельный модуль.
     * @param string $slug Уникальный идентификатор модуля.
     * @param string $path Относительный путь к главному файлу модуля.
     * @param string $class_name Ожидаемое имя главного класса модуля.
     */
    private function load_module($slug, $path, $class_name) {
        $full_path = NPK_CALCULATORS_PATH . $path;
        
        if (!file_exists($full_path)) {
            error_log("NPK Calculators: Модуль '{$slug}' не найден по пути: {$full_path}");
            return;
        }
        
        require_once $full_path;
        
        if (!class_exists($class_name)) {
            error_log("NPK Calculators: Класс модуля '{$class_name}' не найден.");
            return;
        }
        
        // Инициализируем модуль
        if (method_exists($class_name, 'init')) {
            call_user_func([$class_name, 'init']);
        } else {
            error_log("NPK Calculators: Модуль '{$class_name}' не имеет метода init().");
        }
    }
    
    /**
     * Проверяет, выполнены ли все требования для работы плагина.
     * @return bool
     */
    private function are_requirements_met() {
        $wp_ok = version_compare(get_bloginfo('version'), self::MIN_WP_VERSION, '>=');
        $php_ok = version_compare(PHP_VERSION, self::MIN_PHP_VERSION, '>=');
        $db_ok = class_exists('NPK_Database');
        
        return $wp_ok && $php_ok && $db_ok;
    }
    
    /**
     * Действия при активации плагина.
     */
    public function on_activation() {
        // Проверяем требования перед активацией
        if (!$this->are_requirements_met()) {
            deactivate_plugins(plugin_basename(__FILE__));
            wp_die(__('NPK Calculators не может быть активирован из-за несоответствия требованиям.', 'npk-calculators'));
        }
        
        // Можно добавить создание таблиц БД или опций
    }
    
    /**
     * Действия при деактивации плагина.
     */
    public function on_deactivation() {
        // Очистка временных данных
    }
    
    /**
     * Загружает переводы.
     */
    public function load_textdomain() {
        load_plugin_textdomain(
            'npk-calculators',
            false,
            dirname(plugin_basename(__FILE__)) . '/languages/'
        );
    }
}

// Инициализация плагина
add_action('plugins_loaded', ['NPK_Calculators', 'get_instance'], 1);

// Глобальная функция для доступа
if (!function_exists('npk_calculators')) {
    function npk_calculators() {
        return NPK_Calculators::get_instance();
    }
}
```

## 🔗 ЗАВИСИМОСТИ И ДАННЫЕ
- **Загружается:** На хуке WordPress `plugins_loaded` с приоритетом 1.
- **Инициализирует модули:** На хуке `plugins_loaded` с приоритетом 20.
- **Использует файлы:** `shared/config/npk-database.php`, `module-concentration/concentration-module.php`.

## ⚠️ КРИТИЧЕСКИЕ ТРЕБОВАНИЯ (нарушение = неприемка)
1.  **ЗАГРУЗКА ТОЛЬКО НА ХУКЕ:** Модули должны инициализироваться **исключительно** в методе `init_modules()`, который вызывается на хуке `plugins_loaded`.
2.  **ПОРЯДОК ИНИЦИАЛИЗАЦИИ:** Сначала проверка требований (5), потом БД (10), потом переводы (15), потом модули (20).
3.  **ЦЕНТРАЛИЗОВАННОЕ УПРАВЛЕНИЕ:** Все модули загружаются через единый метод `load_module()`. Не должно быть прямых `require_once` вне этого метода.
4.  **ОБРАБОТКА ОШИБОК:** Если модуль не найден или его класс отсутствует — ошибка должна логироваться, но не ломать загрузку других модулей.

## 📝 ОСОБЫЕ УКАЗАНИЯ
```php
// Правильный порядок приоритетов на хуке plugins_loaded:
// 1  - get_instance() плагина
// 5  - check_requirements()
// 10 - include_database()
// 15 - load_textdomain()
// 20 - init_modules() ← здесь шорткоды регистрируются

// Модуль концентрации должен быть изменён:
// В concentration-module.php УБРАТЬ строку:
// add_action('plugins_loaded', ['NPK_Concentration_Module', 'init']);
// Так как теперь модуль инициализируется вызовом NPK_Concentration_Module::init() из load_module()
```

---
