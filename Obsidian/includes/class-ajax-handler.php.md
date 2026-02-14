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
  - "[[Схема_5 Поэтапного создания плагина]]"
Категория:
Дата:
---
Принят



<?php
/**
 * AJAX Handler for Fertilizer Selector Module
 * 
 * @package NPK_Fertilizer_Selector
 */

// Предотвращение прямого доступа
if (!defined('ABSPATH')) {
    exit;
}

// Подключение необходимых классов
require_once __DIR__ . '/class-cache-manager.php';
require_once __DIR__ . '/class-selection-engine.php';

class NPK_AJAX_Handler {
    
    /**
     * Constructor
     */
    public function __construct() {
        // Регистрация AJAX-хуков
        add_action('wp_ajax_npk_fertilizer_select', [$this, 'handle_fertilizer_select']);
        add_action('wp_ajax_nopriv_npk_fertilizer_select', [$this, 'handle_fertilizer_select']);
    }
    
    /**
     * Handle fertilizer selection AJAX request
     */
    public function handle_fertilizer_select(): void {
        // Проверка nonce
        if (!check_ajax_referer('npk_fertilizer_nonce', 'nonce', false)) {
            wp_send_json_error('Ошибка безопасности: недействительный nonce');
            return;
        }
        
        // Получение данных из POST (FormData)
        $crop_id = isset($_POST['crop_id']) ? sanitize_text_field($_POST['crop_id']) : '';
        $phase_id = isset($_POST['phase_id']) ? sanitize_text_field($_POST['phase_id']) : '';
        
        // Валидация обязательных полей
        if (empty($crop_id) || empty($phase_id)) {
            wp_send_json_error('Отсутствуют обязательные параметры: crop_id и phase_id');
            return;
        }
        
        // Генерация ключа кеша
        $cache_key = "rec_{$crop_id}_{$phase_id}";
        
        // Создание экземпляра кеш-менеджера
        $cache = new NPK_Cache_Manager();
        
        // Проверка кеша ДО расчёта
        $cached_results = $cache->get($cache_key);
        
        if (null !== $cached_results) {
            // Возврат кешированных результатов
            wp_send_json_success(['recommendations' => $cached_results]);
            return;
        }
        
        // Вызов Selection Engine для расчёта
        $results = NPK_Selection_Engine::find_matching_fertilizers($crop_id, $phase_id);
        
        // Сохранение результатов в кеш
        $cache->set($cache_key, $results);
        
        // Отправка успешного ответа
        wp_send_json_success(['recommendations' => $results]);
    }
}


// ============================================
// КОНЕЦ ФАЙЛА: module-fertilizer-selector/includes/class-ajax-handler.php
// ============================================