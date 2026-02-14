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


Проверен, принят


```php



<?php
/**
 * Класс-адаптер для подготовки данных к передаче в JavaScript
 * 
 * @package NPK_Fertilizer_Selector
 * @subpackage Includes
 */

// Запрет прямого вызова
if (!defined('ABSPATH')) {
    exit;
}

// Подключение класса кеш-менеджера
require_once __DIR__ . '/class-cache-manager.php';

/**
 * Класс NPK_Data_Adapter
 * 
 * Отвечает только за подготовку справочных данных для wp_localize_script().
 * НЕ участвует в расчётах и НЕ вызывает Selection_Engine.
 */
class NPK_Data_Adapter {

    /**
     * Экземпляр кеш-менеджера
     *
     * @var NPK_Cache_Manager
     */
    private $cache;

    /**
     * Конструктор класса
     */
    public function __construct() {
        $this->cache = new NPK_Cache_Manager();
    }

    /**
     * Подготавливает массив данных для локализации скрипта
     *
     * @return array Массив с данными: crops, fertilizers, ajax_url, nonce
     */
    public function get_localized_data(): array {
        // Получение культур с использованием кеша
        $crops = $this->cache->get('all_crops');
        if (null === $crops) {
            $crops = NPK_Database::get_crops();
            $this->cache->set('all_crops', $crops);
        }

        // Получение минимальных данных удобрений с использованием кеша
        $fertilizers = $this->cache->get('all_fertilizers_minimal');
        if (null === $fertilizers) {
            $fertilizers = NPK_Database::get_fertilizers();
            $this->cache->set('all_fertilizers_minimal', $fertilizers);
        }

        // Базовый массив с данными
        $data = [
            'crops'       => $crops,
            'fertilizers' => $fertilizers,
            'ajax_url'    => admin_url('admin-ajax.php'),
            'nonce'       => wp_create_nonce('npk_selector_nonce'),
        ];

        // Применяем стандартные WordPress-аргументы (на будущее)
        return wp_parse_args($data, [
            'crops'       => [],
            'fertilizers' => [],
            'ajax_url'    => '',
            'nonce'       => '',
        ]);
    }
}

