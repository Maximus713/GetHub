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



<?php
/**
 * Класс для управления кешированием результатов подбора удобрений
 * 
 * @package NPK_Fertilizer_Selector
 * @subpackage Includes
 */

class NPK_Cache_Manager {
    
    /**
     * Префикс для ключей кеша
     *
     * @var string
     */
    protected $prefix = 'npk_fert_rec_';
    
    /**
     * Получить данные из кеша
     *
     * @param string $key Ключ кеша (без префикса)
     * @return mixed Данные из кеша или false если кеш не найден
     */
    public function get(string $key) {
        return get_transient($this->prefix . $key);
    }
    
    /**
     * Сохранить данные в кеш
     *
     * @param string $key Ключ кеша (без префикса)
     * @param mixed $data Данные для сохранения
     * @param int $ttl Время жизни кеша в секундах (по умолчанию HOUR_IN_SECONDS)
     * @return bool Результат сохранения
     */
    public function set(string $key, $data, int $ttl = HOUR_IN_SECONDS): bool {
        return set_transient($this->prefix . $key, $data, $ttl);
    }
    
    /**
     * Удалить данные из кеша
     *
     * @param string $key Ключ кеша (без префикса)
     * @return bool Результат удаления
     */
    public function delete(string $key): bool {
        return delete_transient($this->prefix . $key);
    }
}
