---
tags:
  - Ledgreen_ru
  - Калькулятор
  - AI
  - Подбор_удобрений
Похожее:
Сфера:
  - "[[Бизнес]]"
  - "[[IT]]"
  - "[[Web]]"
Источник:
Ссылки:
  - "[[Полезное]]"
  - "[[Комплексный калькулятор удобрений]]"
  - "[[Подбор удобрений]]"
  - "[[Калькулятор концентрации]]"
Категория:
Дата:
---
Проверен. Ошибок нет! 

<?php
/**
 * Fertilizer Selector Module
 *
 * Главный файл модуля подбора удобрений.
 * Регистрирует модуль в ядре WordPress и подключает зависимости.
 *
 * @package NPK_Fertilizer_Selector
 */

// Защита от прямого доступа
defined('ABSPATH') || exit;

/**
 * Подключение классов модуля в порядке зависимостей
 */
require_once __DIR__ . '/includes/class-selection-engine.php';
require_once __DIR__ . '/includes/class-cache-manager.php';
require_once __DIR__ . '/includes/class-data-adapter.php';
require_once __DIR__ . '/includes/class-ajax-handler.php';
require_once __DIR__ . '/includes/class-shortcode.php';

/**
 * Инициализация модуля подбора удобрений
 * 
 * @return void
 */
function npk_fertilizer_selector_init() {
    // Регистрация классов модуля
    NPK_Fertilizer_Selection_Engine::init();
    NPK_Fertilizer_Cache_Manager::init();
    NPK_Fertilizer_Data_Adapter::init();
    NPK_Fertilizer_Ajax_Handler::init();
    NPK_Fertilizer_Shortcode::init();
}

// Хук инициализации с приоритетом 5 для ранней загрузки
add_action('plugins_loaded', 'npk_fertilizer_selector_init', 5);

/**
 * Хуки активации/деактивации модуля
 */
register_activation_hook(__FILE__, function() {
    // Очистка кэша при активации
    if (class_exists('NPK_Fertilizer_Cache_Manager')) {
        NPK_Fertilizer_Cache_Manager::clear_cache();
    }
});

register_deactivation_hook(__FILE__, function() {
    // Очистка кэша при деактивации
    if (class_exists('NPK_Fertilizer_Cache_Manager')) {
        NPK_Fertilizer_Cache_Manager::clear_cache();
    }
});