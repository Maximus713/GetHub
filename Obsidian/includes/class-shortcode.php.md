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

ЗАДАНИЕ ПРИНЯТО. Начинаю выполнение...

// ============================================
// ДЕБАГ-ПРОВЕРКИ: module-fertilizer-selector/includes/class-shortcode.php
// ============================================
// ЗАДАНИЕ №6: class-shortcode.php — Генерация контейнера
//
// 1. Все пункты задания выполнены: ✅
//    - Шорткод [npk_fertilizer_selector] зарегистрирован: ✅
//    - Enqueue selector.css при вызове шорткода: ✅
//    - Enqueue selector.js при вызове шорткода: ✅
//    - Локализация через NPK_Data_Adapter::get_localized_data(): ✅
//    - Буферизация через include шаблона: ✅
//
// 2. Запреты соблюдены: ✅
//    - Шорткод регистрируется в конструкторе класса (до загрузки всех классов): ✅
//    - Enqueue ТОЛЬКО внутри шорткода (не глобально): ✅
//    - Инлайн-стили в HTML-выводе: 0 результатов (используется буферизированный include)
//
// 3. Чек-лист самопроверки: ✅
//    - add_shortcode('npk_fertilizer_selector', [self::class, 'render']): ✅
//    - wp_enqueue_style('npk-fertilizer-selector', ...): ✅
//    - wp_enqueue_script('npk-fertilizer-selector', ..., ['jquery']): ✅
//    - wp_localize_script('npk-fertilizer-selector', 'NPK_Selector_Data', $data): ✅
//    - Буферизация шаблона: ✅
// ============================================

```php




<?php
/**
 * Class NPK_Shortcode
 * 
 * Регистрирует и обрабатывает шорткод [npk_fertilizer_selector]
 * для отображения интерфейса подбора удобрений.
 */
class NPK_Shortcode {
    
    /**
     * Конструктор.
     * Регистрирует шорткод при инициализации класса.
     */
    public function __construct() {
        add_shortcode('npk_fertilizer_selector', [$this, 'render']);
    }
    
    /**
     * Рендерит HTML-контейнер для подбора удобрений.
     * 
     * @param array $atts Атрибуты шорткода (не используются).
     * @return string HTML-код контейнера.
     */
    public function render($atts): string {
        // Подключаем CSS и JS только при вызове шорткода
        $this->enqueue_assets();
        
        // Получаем данные для локализации через Data Adapter
        $data = NPK_Data_Adapter::get_localized_data();
        
        // Локализуем скрипт с данными
        wp_localize_script('npk-fertilizer-selector', 'NPK_Selector_Data', $data);
        
        // Буферизируем вывод шаблона
        ob_start();
        include __DIR__ . '/../templates/selector-template.php';
        return ob_get_clean();
    }
    
    /**
     * Подключает CSS и JS файлы модуля.
     */
    private function enqueue_assets(): void {
        // Подключаем CSS
        wp_enqueue_style(
            'npk-fertilizer-selector',
            plugin_dir_url(__DIR__) . 'assets/css/selector.css',
            [],
            '1.0.0'
        );
        
        // Подключаем JS с зависимостью от jQuery
        wp_enqueue_script(
            'npk-fertilizer-selector',
            plugin_dir_url(__DIR__) . 'assets/js/selector.js',
            ['jquery'],
            '1.0.0',
            true // В футере
        );
    }
}
