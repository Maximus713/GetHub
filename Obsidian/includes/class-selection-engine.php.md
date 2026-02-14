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


Проверен. Приянтт


<?php
/**
 * Selection Engine для модуля подбора удобрений
 * Реализует алгоритм Отиаи для расчёта процента соответствия
 * 
 * @package NPK_Fertilizer_Selector
 * @subpackage Includes
 */

class NPK_Selection_Engine {
    
    /**
     * CSS-цвета для прогресс-баров согласно спецификации Модуля 2
     */
    const PROGRESS_BAR_COLORS = [
        'green' => '#28a745',  // ≥85%
        'orange' => '#FFD700', // 75-84%
        'grey' => '#6c757d'    // <75% или fallback
    ];
    
    /**
     * Находит удобрения, соответствующие требованиям культуры и фазы
     * 
     * @param string $crop_id  ID культуры
     * @param string $phase_id ID фазы вегетации
     * @return array Массив удобрений с процентами соответствия, подсказками, цветовой классификацией и CSS-цветами для прогресс-баров
     */
    public function find_matching_fertilizers(string $crop_id, string $phase_id): array {
        // Получаем требования для культуры и фазы
        $requirements = NPK_Database::get_crop_phase_requirements($crop_id, $phase_id);
        if (!$requirements) {
            return [];
        }
        
        $target_npk = $requirements['npk'];
        $priority_additives = $requirements['priority'] ?? [];
        $phase_name = $requirements['phase_name'] ?? '';
        
        // Получаем все удобрения и константы
        $fertilizers = NPK_Database::get_fertilizers();
        $constants = NPK_Database::get_constants();
        
        $threshold = $constants['thresholds']['min_match_percentage'];
        $green_threshold = $constants['colors']['green_threshold'];
        $orange_range = $constants['colors']['orange_range'];
        
        $results = [];
        
        foreach ($fertilizers as $fertilizer) {
            // Нормализуем вектора NPK
            $norm_target = NPK_Database::normalize_npk($target_npk);
            $norm_fertilizer = NPK_Database::normalize_npk($fertilizer['npk']);
            
            // Алгоритм Отиаи: поэлементный минимум → сумма → ×100
            $overlap_sum = 0;
            for ($i = 0; $i < 3; $i++) {
                $overlap_sum += min($norm_target[$i], $norm_fertilizer[$i]);
            }
            
            $match_percentage = round($overlap_sum * 100, 1);
            
            // Генерация подсказок: пересечение additives и priority
            $tips = [];
            if (!empty($priority_additives) && !empty($fertilizer['additives'])) {
                $intersection = array_intersect($priority_additives, $fertilizer['additives']);
                foreach ($intersection as $element) {
                    $tips[] = "Содержит {$element}, важный для {$phase_name}";
                }
            }
            
            // Цветовая классификация с использованием констант
            $color = 'grey';
            $progress_color = self::PROGRESS_BAR_COLORS['grey'];
            
            if ($match_percentage >= $green_threshold) {
                $color = 'green';
                $progress_color = self::PROGRESS_BAR_COLORS['green'];
            } elseif ($match_percentage >= $orange_range['min'] && $match_percentage <= $orange_range['max']) {
                $color = 'orange';
                $progress_color = self::PROGRESS_BAR_COLORS['orange'];
            }
            
            $results[] = [
                'id' => $fertilizer['id'],
                'name' => $fertilizer['name'],
                'npk' => $fertilizer['npk'],
                'match_percentage' => $match_percentage,
                'match_percentage_raw' => $overlap_sum, // Для прогресс-бара (0-1)
                'tips' => $tips,
                'color' => $color,
                'progress_color' => $progress_color, // HEX-цвет для CSS
                'progress_class' => 'npk-progress-' . $color, // CSS-класс для прогресс-бара
                'fallback' => false
            ];
        }
        
        // Сортируем по проценту соответствия (убывание)
        usort($results, fn($a, $b) => 
            $b['match_percentage'] <=> $a['match_percentage'] ?: strcmp($a['name'], $b['name'])
        );
        
        // Отбираем проходные (≥ порога)
        $passing = array_filter($results, fn($item) => $item['match_percentage'] >= $threshold);
        
        // Fallback-режим: если нет проходных, берём 3 лучших
        if (count($passing) === 0) {
            $fallback = array_slice($results, 0, 3);
            foreach ($fallback as &$item) {
                $item['fallback'] = true;
                $item['color'] = 'grey';
                $item['progress_color'] = self::PROGRESS_BAR_COLORS['grey'];
                $item['progress_class'] = 'npk-progress-grey';
            }
            return $fallback;
        }
        
        return array_values($passing);
    }
    
    /**
     * Возвращает CSS-цвета для прогресс-баров
     * Используется в JS для динамического обновления
     * 
     * @return array Массив с CSS-цветами для прогресс-баров
     */
    public static function get_progress_bar_colors(): array {
        return self::PROGRESS_BAR_COLORS;
    }
}

