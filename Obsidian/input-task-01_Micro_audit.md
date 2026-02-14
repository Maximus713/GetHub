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

ТЫ — ОПЕРАЦИОННЫЙ АУДИТОР (MICRO_AUDIT)
[


╔══════════════════════════════════════════════════════════════════════════════╗
║                ТЫ — ОПЕРАЦИОННЫЙ АУДИТОР (MICRO_AUDIT)                     ║
║                                                                            ║
║   КЛЮЧЕВОЙ ВОПРОС: "ПРАВИЛЬНО ЛИ это построено технически?"              ║
║   ТВОЯ МИССИЯ: Проверить код на техническую корректность, безопасность   ║
║                и отсутствие запрещённых паттернов.                        ║
║                                                                            ║
║   ТЫ — ПОСЛЕДНЯЯ ЛИНИЯ ОБОРОНЫ перед интеграцией кода.                   ║
║   ТВОЙ ВЕРДИКТ — БИНАРНЫЙ: ✅ ПРОВЕРЕНО или ❌ ОТКЛОНЕНО.                ║
╚══════════════════════════════════════════════════════════════════════════════╝
────────────────────────────────────────────────────────────────────────────────
📦 ВХОДЯЩИЙ ПАКЕТ (ЧТО ТЫ ПОЛУЧАЕШЬ)
────────────────────────────────────────────────────────────────────────────────
ПАКЕТ = {
  "код": [ИСХОДНЫЙ КОД ФАЙЛА ДЛЯ ПРОВЕРКИ],
  "задание": [ПОЛНЫЙ ТЕКСТ ЗАДАНИЯ ОТ КООРДИНАТОРА],
  "тип_файла": "PHP | JS | CSS",
  "ошибки": [РЕЛЕВАНТНЫЕ ПУНКТЫ ИЗ СПИСКА ИСТОРИЧЕСКИХ ОШИБОК],
  "архитектура": [ТАБЛИЦА ДОПУСТИМЫХ ВЫЗОВОВ + ЗОЛОТОЕ ПРАВИЛО],
  "стандарты": [
    "WordPress Coding Standards",
    "kebab-case для имён файлов",
    "Префикс .npk- для CSS",
    "Префикс data-npk- для data-атрибутов"
  ],
  "специфика_модуля": "Модуль 2 — подбор удобрений, алгоритм Отиаи, Instant Preview",
  "задача": "Проверить техническую корректность и безопасность кода"
}
────────────────────────────────────────────────────────────────────────────────
🚫 ЧТО ТЫ НЕ ЗНАЕШЬ (ЖЁСТКАЯ КОНТЕКСТНАЯ ИЗОЛЯЦИЯ)
────────────────────────────────────────────────────────────────────────────────
❌ ТЕБЕ НЕ ДАЮТСЯ (НАМЕРЕННО):
   • Полное ТЗ модуля 2 — ты работаешь ТОЛЬКО с заданием
   • Архитектурный план — ты проверяешь код, а не архитектуру
   • Другие файлы проекта — ты проверяешь ТОЛЬКО конкретный файл в пакете
   • Стратегические решения — «почему так спланировано» не твой уровень
   • Бизнес-ценность модуля — только техническая реализация
❌ ТЫ НЕ ДОЛЖЕН:
   • ДАВАТЬ РЕКОМЕНДАЦИИ ПО ИСПРАВЛЕНИЮ — твоя роль КОНСТАТАЦИЯ, а не КОНСУЛЬТАЦИЯ
   • Предлагать альтернативные архитектурные решения
   • Писать код за кодера — только указать на ошибку
   • Оценивать нефункциональные требования (производительность, UX)
   • Использовать небинарные вердикты («почти готово», «есть нюансы»)
⚠️ ТЫ — ТЕХНИЧЕСКИЙ ИНСПЕКТОР. ТЫ НЕ УЧИШЬ КОДЕРА ПИСАТЬ КОД.
────────────────────────────────────────────────────────────────────────────────
🎯 ТВОИ ЗАДАЧИ — 5 КРИТЕРИЕВ ПРОВЕРКИ
────────────────────────────────────────────────────────────────────────────────
┌─────────────────────────────────────────────────────────────────────────────┐
│ КРИТЕРИЙ 1: ТЕХНИЧЕСКАЯ КОРРЕКТНОСТЬ                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│ 🔲 Синтаксических ошибок нет (PHP/JS/CSS)                                 │
│ 🔲 Соответствует WordPress Coding Standards                               │
│ 🔲 Type hints для параметров и возвращаемых значений (PHP)                │
│ 🔲 Документирующие комментарии (DocBlocks)                                │
│ 🔲 Нет дебаг-выводов (var_dump, error_log, console.log)                   │
└─────────────────────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────────────────────┐
│ КРИТЕРИЙ 2: СООТВЕТСТВИЕ ЗАДАНИЮ                                          │
├─────────────────────────────────────────────────────────────────────────────┤
│ 🔲 Все пункты задания выполнены                                           │
│ 🔲 Все пункты чек-листа самопроверки подтверждены                         │
│ 🔲 Нет лишнего функционала (сверх задания)                                │
│ 🔲 Имена файлов и путей соответствуют заданию (kebab-case)                │
└─────────────────────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────────────────────┐
│ КРИТЕРИЙ 3: ОТСУТСТВИЕ ЗАПРЕЩЁННЫХ ПАТТЕРНОВ                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                        🔴 КРИТИЧЕСКИЕ ЗАПРЕТЫ 🔴                          │
│         (НЕМЕДЛЕННОЕ ОТКЛОНЕНИЕ ПРИ ОБНАРУЖЕНИИ)                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│ AJAX в Модуле 2:                                                          │
│ 🔲 Поиск "URLSearchParams" → 0 результатов                                │
│ 🔲 Поиск "application/json" → 0 результатов                               │
│ 🔲 Поиск "Content-Type: application/json" → 0 результатов                 │
│ 🔲 Используется FormData() — ✅                                            │
│ 🔲 Content-Type НЕ указывается — ✅                                        │
│                                                                             │
│ Стили:                                                                    │
│ 🔲 Поиск "style\s*=" в HTML/шаблонах → 0 результатов                      │
│ 🔲 Поиск ".style." в JS → ТОЛЬКО width/height/transform                    │
│ 🔲 Все статические стили ТОЛЬКО в .css файле                              │
│                                                                             │
│ Имена и селекторы:                                                        │
│ 🔲 Имена файлов: kebab-case (нет заглавных)                               │
│ 🔲 Классы PHP: префикс NPK_                                               │
│ 🔲 CSS селекторы: префикс .npk- (нет глобальных div/button)               │
│ 🔲 Data-атрибуты: префикс data-npk-                                       │
│                                                                             │
│ Алгоритм Отиаи:                                                           │
│ 🔲 Используется NPK_Database::normalize_npk() — ❌ собственная реализация  │
│ 🔲 Нет прямых SQL-запросов — ТОЛЬКО через NPK_Database::                  │
└─────────────────────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────────────────────┐
│ КРИТЕРИЙ 4: СПЕЦИФИКА МОДУЛЯ 2                                           │
├─────────────────────────────────────────────────────────────────────────────┤
│ 🔲 Алгоритм Отиаи: нормализация → минимумы → сумма → ×100                 │
│ 🔲 Порог 75% взят из NPK_Database::get_constants()                        │
│ 🔲 Fallback-режим: при match_percentage < 75% → 3 лучших с fallback=true  │
│ 🔲 Подсказки: пересечение additives удобрения ∩ priority фазы             │
│ 🔲 Instant Preview: предзагруженные данные, замена ответом сервера        │
│ 🔲 UI обновления в requestAnimationFrame() — НЕТ прямых стилей вне rAF    │
│ 🔲 Отмена AJAX: AbortController используется                               │
│ 🔲 ДЕБАУНС: ❌ НЕ ИСПОЛЬЗОВАТЬ — для Модуля 2 требуется мгновенная реакция│
│ 🔲 Прогресс-бары: цвета из CSS (зелёный #28a745 ≥85%,                     │
│                  оранжевый #FFD700 75-84%, серый #6c757d <75%)            │
└─────────────────────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────────────────────┐
│ КРИТЕРИЙ 5: АРХИТЕКТУРНАЯ ЦЕЛОСТНОСТЬ (НА УРОВНЕ ФАЙЛА)                 │
├─────────────────────────────────────────────────────────────────────────────┤
│ Проверка таблицы допустимых вызовов:                                      │
│                                                                             │
│ 🔲 Selection_Engine: вызывает ТОЛЬКО NPK_Database::*                       │
│ 🔲 Data_Adapter: ТОЛЬКО wp_localize_script(), НЕ участвует в расчётах     │
│ 🔲 AJAX_Handler: НЕ содержит логики расчёта — только передача             │
│ 🔲 Shortcode: НЕ знает формул — только HTML                               │
│ 🔲 JS: ТОЛЬКО UX логика, НЕ источник истины для финальных данных          │
└─────────────────────────────────────────────────────────────────────────────┘
────────────────────────────────────────────────────────────────────────────────
📋 КРИТЕРИИ КАЧЕСТВА ТВОЕЙ ПРОВЕРКИ
────────────────────────────────────────────────────────────────────────────────
✅ ТЕХНИЧЕСКАЯ КОРРЕКТНОСТЬ:   0 синтаксических ошибок
✅ СООТВЕТСТВИЕ ЗАДАНИЮ:        Код делает ТОЧНО то, что требовалось
✅ ЗАПРЕЩЁННЫЕ ПАТТЕРНЫ:        0 нарушений критических запретов
✅ СПЕЦИФИКА МОДУЛЯ 2:          Все пункты проверены
✅ АРХИТЕКТУРНАЯ ЦЕЛОСТНОСТЬ:  Таблица вызовов не нарушена
✅ БИНАРНОСТЬ:                 Только ✅ или ❌, никаких «почти»
✅ ДОКАЗАТЕЛЬСТВА:            Каждое нарушение — цитата кода + строка
────────────────────────────────────────────────────────────────────────────────
🔍 ДЕБАГ-ПРОВЕРКА (СДЕЛАЙ ЭТО ПЕРЕД ВЫВОДОМ КАЖДОГО ВЕРДИКТА)
────────────────────────────────────────────────────────────────────────────────
// ============================================
// ДЕБАГ-ПРОВЕРКИ ОПЕРАЦИОННОГО АУДИТОРА
// ============================================
// 1. Проверен синтаксис:                             [✅/❌]
// 2. Проверены ВСЕ требования задания:               [✅/❌] (N из N)
// 3. Проверены ВСЕ критические запреты:              [✅/❌] (M из M)
//   • URLSearchParams: 0                             [✅/❌]
//   • JSON в AJAX: 0                                 [✅/❌]
//   • Статические стили вне CSS: 0                  [✅/❌]
// 4. Специфика Модуля 2 проверена:                  [✅/❌]
// 5. Архитектурная целостность проверена:           [✅/❌]
// 6. Вердикт строго бинарный:                       [✅/❌]
// 7. НИ ОДНОЙ рекомендации по исправлению:          [✅/❌]
// ============================================
────────────────────────────────────────────────────────────────────────────────
📤 ФОРМАТ ВЕРДИКТА (СТРОГО ОБЯЗАТЕЛЕН)
────────────────────────────────────────────────────────────────────────────────
╔══════════════════════════════════════════════════════════════════════════════╗
║              ОПЕРАЦИОННЫЙ АУДИТ: ВЕРДИКТ ПО КОДУ                           ║
╚══════════════════════════════════════════════════════════════════════════════╝
ВЕРДИКТ: ✅ ПРОВЕРЕНО / ❌ ОТКЛОНЕНО
────────────────────────────────────────────────────────────────────────────────
ФАЙЛ:     [путь/к/файлу]
ЗАДАНИЕ:  №[N] — [название задания]
────────────────────────────────────────────────────────────────────────────────
1. ТЕХНИЧЕСКАЯ КОРРЕКТНОСТЬ
   ✅ Синтаксис: корректный
   ✅ WordPress Coding Standards: соблюдены
   ✅ Type hints: [есть/нет]
   🔸 [Другие пункты]
2. СООТВЕТСТВИЕ ЗАДАНИЮ
   ✅ [Требование 1]: [цитата из кода, строка N]
   ✅ [Требование 2]: [цитата из кода, строка M]
   ❌ [Требование 3]: НЕ РЕАЛИЗОВАНО
      Ожидалось: [цитата из задания]
      Найдено: отсутствует в коде
3. КРИТИЧЕСКИЕ ЗАПРЕТЫ
   ✅ URLSearchParams: 0 результатов
   ✅ JSON в AJAX: 0 результатов
   ✅ Статические стили в JS: 0 нарушений
   ❌ Инлайн-стили в HTML: НАРУШЕНИЕ!
      Строка 42: <div style="padding: 10px;">
      Запрет: Ошибка 10 — все статические стили в CSS
4. СПЕЦИФИКА МОДУЛЯ 2
   ✅ Алгоритм Отиаи: нормализация через normalize_npk() — строка 15
   ✅ Fallback-режим: есть при count < 1 — строка 34
   ✅ Instant Preview: предзагруженные данные — строка 67
   ✅ requestAnimationFrame(): используется — строка 89
   ❌ Дебаунс: ОБНАРУЖЕН — строка 102, setTimeout 300ms
      Требование: для Модуля 2 дебаунс НЕ ИСПОЛЬЗОВАТЬ (мгновенная реакция)
5. АРХИТЕКТУРНАЯ ЦЕЛОСТНОСТЬ
   ✅ Selection_Engine: вызывает только NPK_Database::
   ✅ Data_Adapter: НЕ участвует в расчётах
────────────────────────────────────────────────────────────────────────────────
ИТОГОВОЕ ЗАКЛЮЧЕНИЕ
────────────────────────────────────────────────────────────────────────────────
✅ ПРОВЕРЕНО — Код полностью соответствует заданию,
               не содержит запрещённых конструкций,
               специфика Модуля 2 соблюдена.
               [0 нарушений]
ИЛИ
❌ ОТКЛОНЕНО — Обнаружены КРИТИЧЕСКИЕ нарушения:
               • [нарушение 1] — строка [N]
               • [нарушение 2] — строка [M]
               
               Код НЕ МОЖЕТ быть принят до устранения нарушений.
────────────────────────────────────────────────────────────────────────────────
────────────────────────────────────────────────────────────────────────────────
🚨 ПРИМЕР ВЕРДИКТА (для справки)
────────────────────────────────────────────────────────────────────────────────
ВЕРДИКТ: ❌ ОТКЛОНЕНО
ФАЙЛ: module-fertilizer-selector/assets/js/selector.js
──────────────────────────────────────────────────
6. КРИТИЧЕСКИЕ ЗАПРЕТЫ
   ❌ JSON в AJAX — НАРУШЕНИЕ!
      Строка 52: headers: { 'Content-Type': 'application/json' }
      Запрет: Ошибка 4 — Модуль 2: ТОЛЬКО FormData(), НЕ JSON!
      Должно быть: НЕ указывать Content-Type, body: formData
   
   ❌ Отмена запросов — НАРУШЕНИЕ!
      Строка 48-60: AbortController не используется
      Запрет: Ошибка 7 — требуется AbortController для отмены предыдущих запросов
────────────────────────────────────────────────────────────────────────────────
🚨 ЗАПРЕЩЕНО (ДЛЯ ТЕБЯ — ОПЕРАЦИОННОГО АУДИТОРА)
────────────────────────────────────────────────────────────────────────────────
❌ НЕЛЬЗЯ давать рекомендации по исправлению («нужно заменить X на Y»)
❌ НЕЛЬЗЯ использовать небинарные вердикты («почти готово», «в целом ок»)
❌ НЕЛЬЗЯ пропускать критические запреты (URLSearchParams, JSON, стили)
❌ НЕЛЬЗЯ проверять то, что не входит в задание
❌ НЕЛЬЗЯ принимать код с дебаунсом в Модуле 2
❌ НЕЛЬЗЯ предполагать, что Кодер знал что-то вне контекста задания
────────────────────────────────────────────────────────────────────────────────
✅ ГОТОВ? НАЧНИ СВОЙ ОТВЕТ С:
────────────────────────────────────────────────────────────────────────────────
«ПАКЕТ ПРИНЯТ. Начинаю операционный аудит файла [имя]...»
]

ПАКЕТ = {
  "код": [






// ============================================
```javascript
/**
 * NPK Fertilizer Selector - Module 2
 * 
 * Dependencies: WP jQuery (for AJAX), NPK_Selector_Data global
 * 
 * Features:
 * - Instant Preview (<100ms) using Otiai algorithm with server-side normalization
 * - AbortController for race condition prevention
 * - requestAnimationFrame for DOM updates
 * - FormData AJAX (no JSON)
 * - Color thresholds from PHP constants (no hardcoding)
 */

(function($) {
    'use strict';

    class NPKSelector {
        constructor() {
            this.data = window.NPK_Selector_Data || {};
            this.constants = this.data.constants || { 
                thresholds: { 
                    min_match_percentage: 75,
                    color_thresholds: {
                        green: 85,    // ≥85% - from PHP constants
                        orange: 75    // 75-84% - from PHP constants
                    }
                } 
            };
            this.threshold = this.constants.thresholds.min_match_percentage;
            this.colorThresholds = this.constants.thresholds.color_thresholds;
            
            this.controller = null;
            this.rafId = null;
            
            this.elements = {
                crop: $('#npk-crop-select'),
                phase: $('#npk-phase-select'),
                fertilizer: $('#npk-fertilizer-select'),
                results: $('#npk-results-container'),
                preview: $('#npk-preview-container'),
                concentration1: $('#npk-concentration-1'),
                concentration2: $('#npk-concentration-2')
            };
            
            this.init();
        }

        init() {
            if (!this.data.ajax_url) return;
            
            // Bind events
            this.elements.crop.on('change', this.handleCropChange.bind(this));
            this.elements.phase.on('change', this.handlePhaseChange.bind(this));
            this.elements.fertilizer.on('change', this.handleFertilizerChange.bind(this));
            
            // Initial preview if data exists
            if (this.data.preview_data) {
                this.updatePreview(this.data.preview_data);
            }
        }

        /**
         * Get color class based on match percentage using PHP constants
         */
        getColorClass(percentage) {
            if (percentage >= this.colorThresholds.green) {
                return 'npk-progress-green';
            } else if (percentage >= this.colorThresholds.orange) {
                return 'npk-progress-orange';
            }
            return 'npk-progress-gray';
        }

        /**
         * Instant Preview (<100ms) using Otiai algorithm
         * Note: Normalization is done on server via NPK_Database::normalize_npk()
         */
        calculateInstantPreview(targetNpk, fertilizers) {
            if (!targetNpk || !fertilizers || !fertilizers.length) return [];
            
            const results = [];
            
            for (const fert of fertilizers) {
                // Convert percentages to decimals
                const fertRelative = fert.npk.map(v => v / 100);
                
                // Use pre-normalized values from server (NPK_Database::normalize_npk)
                const normTarget = this.data.normalized_npk?.[targetNpk.join('-')] || this.approximateNormalize(targetNpk);
                const normFert = this.data.normalized_npk?.[fert.npk.join('-')] || this.approximateNormalize(fertRelative);
                
                // Calculate overlap (sum of minimums)
                let overlap = 0;
                for (let i = 0; i < 3; i++) {
                    overlap += Math.min(normTarget[i], normFert[i]);
                }
                
                // Convert to percentage
                const percentage = Math.round(overlap * 100 * 10) / 10;
                
                results.push({
                    id: fert.id,
                    name: fert.name,
                    npk: fert.npk,
                    match_percentage: percentage,
                    tips: fert.tips || []
                });
            }
            
            // Sort by match percentage descending
            results.sort((a, b) => b.match_percentage - a.match_percentage);
            
            return results;
        }

        /**
         * Approximate normalization for Instant Preview only
         * Falls back to this if server-normalized values aren't available
         * Note: In production, NPK_Database::normalize_npk() is source of truth
         */
        approximateNormalize(npk) {
            const sum = npk.reduce((a, b) => a + b, 0);
            if (sum === 0) return [0, 0, 0];
            return npk.map(v => v / sum);
        }

        /**
         * Handle crop selection change
         */
        handleCropChange() {
            const cropId = this.elements.crop.val();
            if (!cropId) return;
            
            // Clear phase select
            this.elements.phase.empty().append('<option value="">Select phase...</option>').prop('disabled', true);
            
            // Load phases via AJAX
            this.fetchPhases(cropId);
        }

        /**
         * Handle phase selection change
         */
        handlePhaseChange() {
            const cropId = this.elements.crop.val();
            const phaseId = this.elements.phase.val();
            
            if (!cropId || !phaseId) return;
            
            // Load requirements and update preview
            this.fetchRequirements(cropId, phaseId);
        }

        /**
         * Handle fertilizer selection change (B.5 - Update concentrations)
         */
        handleFertilizerChange() {
            const fertId = this.elements.fertilizer.val();
            if (!fertId) return;
            
            // Find selected fertilizer data
            const selectedFert = this.findFertilizerById(fertId);
            if (!selectedFert) return;
            
            // Update concentration displays (B.5)
            requestAnimationFrame(() => {
                this.elements.concentration1.text(selectedFert.npk[0] + '%');
                this.elements.concentration2.text(selectedFert.npk[1] + '%');
                // Note: N is usually first, P second, K third - showing N and P as per B.5
            });
            
            // Trigger main AJAX for final calculation
            this.fetchResults(cropId, phaseId, fertId);
        }

        /**
         * Fetch phases for selected crop
         */
        fetchPhases(cropId) {
            this.abortPending();
            this.controller = new AbortController();
            
            const formData = new FormData();
            formData.append('action', 'npk_get_phases');
            formData.append('crop_id', cropId);
            
            $.ajax({
                url: this.data.ajax_url,
                method: 'POST',
                data: formData,
                processData: false,
                contentType: false,
                signal: this.controller.signal,
                success: (response) => {
                    if (response.success && response.data) {
                        this.updatePhaseSelect(response.data);
                    }
                }
                // Error handling removed as per audit - no console.error
            });
        }

        /**
         * Fetch crop requirements and run Instant Preview
         */
        fetchRequirements(cropId, phaseId) {
            this.abortPending();
            this.controller = new AbortController();
            
            const formData = new FormData();
            formData.append('action', 'npk_get_requirements');
            formData.append('crop_id', cropId);
            formData.append('phase_id', phaseId);
            
            // Show loading state
            this.showPreviewLoading();
            
            $.ajax({
                url: this.data.ajax_url,
                method: 'POST',
                data: formData,
                processData: false,
                contentType: false,
                signal: this.controller.signal,
                success: (response) => {
                    if (response.success && response.data) {
                        // Run Instant Preview
                        const previewResults = this.calculateInstantPreview(
                            response.data.npk,
                            this.data.fertilizers || []
                        );
                        
                        // Filter by threshold
                        const filteredResults = previewResults.filter(
                            r => r.match_percentage >= this.threshold
                        );
                        
                        // Update UI via rAF
                        this.updatePreviewResults(filteredResults);
                        
                        // Update fertilizer select
                        this.updateFertilizerSelect(filteredResults);
                    }
                }
                // Error handling removed as per audit - no console.error
            });
        }

        /**
         * Fetch final calculation results
         */
        fetchResults(cropId, phaseId, fertId) {
            this.abortPending();
            this.controller = new AbortController();
            
            const formData = new FormData();
            formData.append('action', 'npk_fertilizer_select');
            formData.append('crop_id', cropId);
            formData.append('phase_id', phaseId);
            formData.append('fertilizer_id', fertId);
            
            $.ajax({
                url: this.data.ajax_url,
                method: 'POST',
                data: formData,
                processData: false,
                contentType: false,
                signal: this.controller.signal,
                success: (response) => {
                    if (response.success && response.data) {
                        // Update final preview (replaces instant preview)
                        this.updatePreview(response.data);
                    }
                }
                // Error handling removed as per audit - no console.error
            });
        }

        /**
         * Abort pending AJAX requests
         */
        abortPending() {
            if (this.controller) {
                this.controller.abort();
                this.controller = null;
            }
        }

        /**
         * Update phase select options
         */
        updatePhaseSelect(phases) {
            requestAnimationFrame(() => {
                const select = this.elements.phase.empty();
                select.append('<option value="">Select phase...</option>');
                
                phases.forEach(phase => {
                    select.append(`<option value="${phase.id}">${phase.name}</option>`);
                });
                
                select.prop('disabled', false);
            });
        }

        /**
         * Update fertilizer select with preview results
         */
        updateFertilizerSelect(results) {
            requestAnimationFrame(() => {
                const select = this.elements.fertilizer.empty();
                select.append('<option value="">Select fertilizer...</option>');
                
                results.forEach(fert => {
                    select.append(
                        `<option value="${fert.id}" data-match="${fert.match_percentage}">` +
                        `${fert.name} (${fert.match_percentage}%)</option>`
                    );
                });
                
                select.prop('disabled', results.length === 0);
            });
        }

        /**
         * Update preview results container
         */
        updatePreviewResults(results) {
            if (this.rafId) {
                cancelAnimationFrame(this.rafId);
            }
            
            this.rafId = requestAnimationFrame(() => {
                const container = this.elements.preview;
                
                if (!results.length) {
                    container.html('<p class="npk-no-results">No fertilizers match the requirements</p>');
                    return;
                }
                
                let html = '<div class="npk-preview-list">';
                results.slice(0, 5).forEach(fert => {
                    // Get color class from PHP constants (no hardcoding)
                    const colorClass = this.getColorClass(fert.match_percentage);
                    
                    html += `
                        <div class="npk-preview-item" data-id="${fert.id}">
                            <div class="npk-preview-header">
                                <span class="npk-preview-name">${fert.name}</span>
                                <span class="npk-preview-match ${colorClass}">${fert.match_percentage}%</span>
                            </div>
                            <div class="npk-preview-npk">
                                N:${fert.npk[0]}% P:${fert.npk[1]}% K:${fert.npk[2]}%
                            </div>
                            ${fert.tips && fert.tips.length ? 
                                `<div class="npk-preview-tips">${fert.tips[0]}</div>` : ''}
                        </div>
                    `;
                });
                html += '</div>';
                
                container.html(html);
                this.rafId = null;
            });
        }

        /**
         * Update final preview with full results
         */
        updatePreview(data) {
            requestAnimationFrame(() => {
                const container = this.elements.preview;
                
                let html = '<div class="npk-result-details">';
                
                if (data.fertilizer) {
                    html += `
                        <h4>Selected Fertilizer</h4>
                        <div class="npk-result-fertilizer">
                            <div class="npk-result-name">${data.fertilizer.name}</div>
                            <div class="npk-result-npk">
                                NPK: ${data.fertilizer.npk.join('-')}
                            </div>
                        </div>
                    `;
                }
                
                if (data.calculation) {
                    html += `
                        <h4>Calculation Details</h4>
                        <div class="npk-result-calculation">
                            <div>Match: ${data.calculation.match_percentage}%</div>
                            ${data.calculation.fallback ? 
                                '<div class="npk-result-fallback">Fallback mode</div>' : ''}
                        </div>
                    `;
                }
                
                if (data.tips && data.tips.length) {
                    html += `
                        <h4>Tips</h4>
                        <ul class="npk-result-tips">
                            ${data.tips.map(tip => `<li>${tip}</li>`).join('')}
                        </ul>
                    `;
                }
                
                html += '</div>';
                container.html(html);
            });
        }

        /**
         * Show loading state in preview
         */
        showPreviewLoading() {
            requestAnimationFrame(() => {
                this.elements.preview.html('<p class="npk-loading">Loading preview...</p>');
            });
        }

        /**
         * Find fertilizer by ID in local data
         */
        findFertilizerById(id) {
            if (!this.data.fertilizers) return null;
            return this.data.fertilizers.find(f => f.id == id);
        }
    }

    // Initialize when DOM is ready
    $(document).ready(() => {
        new NPKSelector();
    });

})(jQuery);
```
// ============================================
// КОНЕЦ ФАЙЛА: module-fertilizer-selector/assets/js/selector.js
// ============================================




  ],
  "задание": [




═══════════════════════════════════════════════════════════════════════════════
ЗАДАНИЕ №9: selector.js — UX/UI и Instant Preview (ФИНАЛ)
═══════════════════════════════════════════════════════════════════════════════

📁 ФАЙЛЫ:
   СОЗДАТЬ: [module-fertilizer-selector/assets/js/selector.js]

📋 ТРЕБОВАНИЯ:
1. Instant Preview (<100мс) — алгоритм Отиаи для UX
2. Фильтрация: ≥ порога (из `NPK_Selector_Data.constants`)
3. AJAX: FormData(), AbortController
4. UI: все обновления через `requestAnimationFrame()`
5. Цвета: добавление классов, НЕ прямые стили
6. Две концентрации (B.5): обновление NPK концентрации при смене удобрения

⛔ ЗАПРЕТЫ:
❌ [Неправильный AJAX] — FormData(), НЕ JSON, НЕ URLSearchParams
❌ [Отмена запросов] — AbortController ОБЯЗАТЕЛЕН
❌ [Моргание UI] — Все DOM-изменения через rAF
❌ [Статические стили] — НЕТ `.style.` (кроме width/height)
❌ [Игнорирование финала] — Preview заменяется ответом сервера

🧪 ЧЕК-ЛИСТ:
[ ] Instant Preview: нормализация → минимумы → сумма → ×100
[ ] Порог из констант: `NPK_Selector_Data.constants.min_match_percentage`
[ ] AJAX: `new FormData()`, `append('action', 'npk_fertilizer_select')`
[ ] AbortController: `if (controller) controller.abort();`
[ ] rAF: `requestAnimationFrame(() => { ... })`
[ ] Цвета: `classList.add('npk-progress-green')`, НЕ `.style.background`
[ ] КТ-3: Instant Preview <100мс

⚠️ ТРИГГЕР АУДИТА: JS — Instant Preview, AJAX, rAF, AbortController

═══════════════════════════════════════════════════════════════════════════════



  ],
  "ошибки": [
  

```markdown
# 📋 **СПИСОК ОШИБОК**

## 🚨 **КРИТИЧЕСКИЕ ОШИБКИ (плагин НЕ РАБОТАЕТ)**

### **ОШИБКА 1: ❌ Неправильные имена файлов**

#### **ФАЙЛЫ (всегда kebab-case):**
```php
❌ npkCalculators.php, concentration_module.php, CalculationEngine.php
✅ npk-calculators.php, concentration-module.php, class-calculation-engine.php
```

#### **CSS селекторы (всегда с префиксом .npk-):**
```css
❌ .calculator-container, .npkCalculatorContainer
✅ .npk-calculator-container, .npk-ground-button
```

#### **Data-атрибуты (всегда с префиксом data-npk-):**
```html
❌ data-calculator-id, dataNpkCalculatorId  
✅ data-npk-calculator-id, data-npk-ground-type
```

---

### **ОШИБКА 2: ❌ Нарушен порядок создания файлов**

**НЕПРАВИЛЬНО (вызовет ошибку 500):**
```
1. concentration-module.php
2. Он пытается подключить class-calculation-engine.php
3. Этого файла ещё нет!
4. Фатальная ошибка PHP
```

**ПРАВИЛЬНО:**
```
✅ 1. npk-calculators.php (главный файл)
✅ 2. npk-database.php (база данных - уже есть)
✅ 3. class-calculation-engine.php (ядро логики)
✅ 4. class-data-adapter.php, class-ajax-handler.php (зависимости)
✅ 5. concentration-module.php (модуль - ПОСЛЕ всех классов!)
✅ 6. Остальные файлы (шаблоны, JS, CSS)
```

**Правило:** Создавай файлы в том порядке, в котором они будут подключаться.

---

### **ОШИБКА 3: ❌ Шорткод не работает**

**НЕПРАВИЛЬНО (пустая страница вместо калькулятора):**
```php
// Слишком поздно
add_action('init', 'init_plugin');

// Регистрация до загрузки классов
function init_plugin() {
    add_shortcode('npk_calculator', 'render_shortcode'); // Классов ещё нет!
    require_once 'class-shortcode.php'; // СЛИШКОМ ПОЗДНО
}
```

**ПРАВИЛЬНО:**
```php
// 1. Ранняя инициализация
add_action('plugins_loaded', 'npk_calculators_init', 5);

function npk_calculators_init() {
    // 2. СНАЧАЛА загружаем ВСЕ классы
    require_once 'class-shortcode.php';
    
    // 3. ПОТОМ регистрируем шорткод
    add_shortcode('npk_concentration_calculator', ['Shortcode', 'render']);
}
```

**Признак ошибки:** `[npk_concentration_calculator]` показывает текст вместо калькулятора.

---

### **ОШИБКА 4: ❌ AJAX вызывает перезагрузку страницы**

**ОШИБКА (Модуль 1):**
```javascript
// ❌ Использование FormData() в Модуле 1 вызывает перезагрузку
const formData = new FormData();
formData.append('action', 'npk_concentration_calculate');
fetch(ajaxurl, { method: 'POST', body: formData });
// Результат: страница перезагружается
```

**✅ ИСПРАВЛЕНИЕ (Модуль 1 — только JSON):**
```javascript
// Правильный формат для Модуля 1
fetch(ajaxurl, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        action: 'npk_concentration_calculate',
        fertilizer_id: 1,
        concentration: 0.3
    })
});
// Результат: AJAX без перезагрузки
```

```php
// PHP для Модуля 1 — получаем JSON
$json_input = file_get_contents('php://input');
$data = json_decode($json_input, true);
wp_send_json_success($result);
```

---

**✅ ПРАВИЛЬНО (Модуль 2 — только FormData()):**
```javascript
// Единственный правильный формат для Модуля 2
const formData = new FormData();
formData.append('action', 'npk_fertilizer_selector_calculate');
formData.append('crop_id', 'tomato');
formData.append('phase_id', 'blooming');

fetch(ajaxurl, {
    method: 'POST',
    body: formData  // ❌ НЕ добавлять Content-Type
});
// Результат: AJAX без перезагрузки
```

```php
// PHP для Модуля 2 — получаем FormData
$crop_id = $_POST['crop_id'] ?? '';
$phase_id = $_POST['phase_id'] ?? '';
wp_send_json_success($result);
```

---

**⚠️ ВАЖНО: НЕ ПУТАТЬ!**
| Модуль | Формат AJAX | Почему |
|--------|------------|--------|
| **Модуль 1** (Концентрации) | ✅ ТОЛЬКО JSON | Кеширование запросов, сложные структуры данных |
| **Модуль 2** (Подбор удобрений) | ✅ ТОЛЬКО FormData() | Архитектурное решение, простота, единообразие |
| **Будущие модули** | ❓ Определяется при классификации | См. ADR-2024-002 |

**Нарушение формата для своего модуля = ОШИБКА.**

---

## ⚠️ **ВАЖНЫЕ ОШИБКИ UX (работает, но плохо)**

### **ОШИБКА 5: ❌ Моргание интерфейса**

**ПЛОХО (моргает):**
```javascript
element.style.width = '50%';
element.classList.add('active');
```

**ЛУЧШЕ (плавно):**
```javascript
requestAnimationFrame(() => {
    element.style.transition = 'width 0.15s ease';
    element.style.width = '50%';
});
```

### **ОШИБКА 6: ❌ Много AJAX запросов**

**ПЛОХО (нагрузка на сервер):**
```javascript
input.addEventListener('input', () => {
    sendAjaxRequest(); // При каждом нажатии клавиши!
});
```

**ЛУЧШЕ (дебаунс 300-500мс):**
```javascript
let timeout;
input.addEventListener('input', () => {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
        sendAjaxRequest(); // Только после паузы
    }, 300);
});
```

### **ОШИБКА 7: ❌ Отсутствие отмены предыдущих AJAX запросов**

**ПЛОХО (race condition):**
```javascript
// Новый запрос не отменяет предыдущий
```

**ЛУЧШЕ:**
```javascript
let controller = null;

async function sendRequest() {
    if (controller) controller.abort(); // Отменяем старый
    controller = new AbortController();
    
    fetch(url, { signal: controller.signal });
}
```

---

## 🔧 **ТЕХНИЧЕСКИЕ ОШИБКИ**

### **ОШИБКА 8: ❌ Не отключил стрелки в полях input[type="number"]**

**ПРОБЛЕМА:** Появляются браузерные стрелки (нарушение ТЗ)

**РЕШЕНИЕ:**
```css
.npk-input-field::-webkit-outer-spin-button,
.npk-input-field::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

.npk-input-field[type="number"] {
    -moz-appearance: textfield;
}
```

### **ОШИБКА 9: ❌ Неправильный z-index и порядок элементов слайдера**

**ПРОБЛЕМА:** Бесцветная полоса (трек перекрывался ползунком)

**РЕШЕНИЕ:** Сначала трек (фон), потом ползунок сверху

---

### **ОШИБКА 10: ❌ Разброс стилей по разным файлам**

**ПРОБЛЕМА (было в Модуле 1):**
- Стили в CSS-файле
- Стили в JavaScript (element.style)
- Стили в HTML (атрибуты style="")
→ Невозможно найти и изменить стили

**РЕШЕНИЕ:**
✅ Все статические стили — ТОЛЬКО в `selector.css`
✅ JavaScript — ТОЛЬКО динамические значения
✅ HTML — минимум инлайн-стилей

**ПРОВЕРКА:**
- В JS нет `element.style.color = "#fff"` (кроме динамических)
- В HTML нет `style="padding: 10px;"` (кроме необходимых)

---

## 🎯 **ОБЯЗАТЕЛЬНЫЕ ПРАВИЛА**

### **ПРАВИЛО 1: СНАЧАЛА КЛАССЫ, ПОТОМ РЕГИСТРАЦИЯ**
```
1. Загрузить ВСЕ классы
2. Зарегистрировать шорткоды
3. Инициализировать модуль
```

### **ПРАВИЛО 2: JSON ДЛЯ AJAX (Модуль 1) / FormData() ДЛЯ AJAX (Модуль 2)**
```
Модуль 1 — ТОЛЬКО JSON
Модуль 2 — ТОЛЬКО FormData()
Не смешивать!
```

### **ПРАВИЛО 3: ПЛАВНЫЙ UI**
```
requestAnimationFrame() для ВСЕХ UI обновлений
Дебаунс для полей ввода
Отмена предыдущих AJAX запросов
```

### **ПРАВИЛО 4: ПРОВЕРКА СВЯЗЕЙ**
```
Перед созданием файла проверь:
1. Все require_once пути существуют
2. Все вызываемые классы/методы существуют
3. Все CSS/ID/селекторы согласованы между файлами
```

---

## ✅ **БЫСТРЫЙ ЧЕК-ЛИСТ ПЕРЕД СДАЧЕЙ:**

### **Проверь эти 4 пункта (критические):**
1. [ ] **Имена файлов:** Все в kebab-case? (`npk-calculators.php`)
2. [ ] **Порядок:** Файлы созданы в правильном порядке? Зависимости существуют?
3. [ ] **Шорткод:** `[npk_concentration_calculator]` показывает калькулятор, а не текст?
4. [ ] **AJAX:** 
   - Модуль 1: используется JSON? 
   - Модуль 2: используется FormData()?
   - Страница не перезагружается?

### **Проверь эти 3 пункта (качество):**
5. [ ] **UI:** Нет морганий при изменении значений? (`requestAnimationFrame`)
6. [ ] **Запросы:** Нет множественных AJAX при быстром вводе? (дебаунс)
7. [ ] **Отмена:** Предыдущие AJAX запросы отменяются? (`AbortController`)

  ],
  "архитектура": [
  

# 📊 ТАБЛИЦА ДОПУСТИМЫХ ВЫЗОВОВ (ДЛЯ ЧАТА 5 — КОДЕР)

## На основе документа «ДОПОЛНЯЮЩЕЕ ТЕХНИЧЕСКОЕ ЗАДАНИЕ»
**Версия:** 1.0  
**Назначение:** Однозначное определение разрешённых и запрещённых вызовов между компонентами системы  
**Статус:** ❌ НАРУШЕНИЕ ЛЮБОГО ПУНКТА = ОШИБКА РЕАЛИЗАЦИИ

---

## 1. ОБЯЗАТЕЛЬНЫЕ ЦЕПОЧКИ (НАРУШЕНИЕ = НЕПРИЁМКА)

| Откуда | Куда | Требование | Цитата из документа |
|--------|------|------------|---------------------|
| **UI (JS)** | → | **AJAX_Handler (PHP)** | «Единственно допустимая цепочка расчёта» | A.1 |
| **AJAX_Handler (PHP)** | → | **Calculation_Engine (PHP)** | «Единственно допустимая цепочка расчёта» | A.1 |
| **Calculation_Engine (PHP)** | → | **NPK_Database (PHP)** | «Единственно допустимая цепочка расчёта» | A.1 |
| **NPK_Database (PHP)** | → | **Calculation_Engine (PHP)** | «Единственно допустимая цепочка расчёта» | A.1 |
| **Calculation_Engine (PHP)** | → | **AJAX_Handler (PHP)** | «Единственно допустимая цепочка расчёта» | A.1 |
| **AJAX_Handler (PHP)** | → | **UI (JS)** | «Единственно допустимая цепочка расчёта» | A.1 |

**✅ ПРИМЕР ПРАВИЛЬНОЙ ЦЕПОЧКИ:**
```php
// AJAX_Handler → Calculation_Engine
$result = Calculation_Engine::calculate($data);
wp_send_json_success($result);
```

**❌ ЛЮБОЕ ОТКЛОНЕНИЕ ОТ ЦЕПОЧКИ — АРХИТЕКТУРНАЯ ОШИБКА**

---

## 2. РАЗРЕШЁННЫЕ ВЫЗОВЫ

| Компонент | Может вызывать | Условия/Ограничения | Цитата |
|-----------|---------------|---------------------|--------|
| **Calculation_Engine** | ✅ **NPK_Database** | Только для получения данных | B.1 |
| **Calculation_Engine** | ✅ **Себя** | Внутренняя логика | B.1 |
| **AJAX_Handler** | ✅ **Calculation_Engine** | Только вызов, НЕ содержит логику | B.1, B.3 |
| **Data_Adapter** | ✅ **NPK_Database** | Только для подготовки данных UI | B.3 |
| **Data_Adapter** | ✅ **wp_localize_script** | Единственное назначение | B.3 |
| **JavaScript (UI)** | ✅ **AJAX_Handler** | Отправка запросов | A.1 |
| **JavaScript (UI)** | ✅ **Свои UX-функции** | Только для мгновенного UX | B.2, D.1 |

---

## 3. ЗАПРЕЩЁННЫЕ ВЫЗОВЫ (КРИТИЧЕСКИЕ ОШИБКИ)

| Компонент | НЕ МОЖЕТ вызывать | Причина | Цитата |
|-----------|-------------------|--------|--------|
| **JavaScript (UI)** | ❌ **Calculation_Engine** | Бизнес-логика ТОЛЬКО на сервере | B.1, B.2 |
| **JavaScript (UI)** | ❌ **NPK_Database** | Прямые запросы к БД запрещены | B.2 |
| **JavaScript (UI)** | ❌ **Себя для финальных решений** | Не является источником истины | B.2, C.1 |
| **Data_Adapter** | ❌ **Calculation_Engine** | Не участвует в расчётах | B.3 |
| **Data_Adapter** | ❌ **AJAX** | Не является частью цепочки расчёта | B.3 |
| **Data_Adapter** | ❌ **Определение state** | State только в Calculation_Engine | B.3, F |
| **AJAX_Handler** | ❌ **NPK_Database** (напрямую) | Только через Calculation_Engine | A.1, B.1 |
| **AJAX_Handler** | ❌ **Бизнес-логика** | Только транспорт | B.1 |
| **Shortcode** | ❌ **Calculation_Engine** | Не имеет права вызывать логику | B.1 |
| **Shortcode** | ❌ **NPK_Database** | Только через Data_Adapter | B.3 |
| **Calculation_Engine** | ❌ **HTML/UI** | Не знает об интерфейсе | B.1 |
| **Calculation_Engine** | ❌ **AJAX** | Не является транспортным слоем | B.1 |
| **Calculation_Engine** | ❌ **wp_localize_script** | Не его ответственность | B.1 |

**❌ ПРИМЕРЫ ЗАПРЕЩЁННЫХ ВЫЗОВОВ:**
```php
// Data_Adapter НЕ МОЖЕТ вызывать Calculation_Engine
class Data_Adapter {
    public function get_data() {
        $result = Calculation_Engine::calculate(); // ❌ ЗАПРЕЩЕНО
    }
}

// AJAX_Handler НЕ МОЖЕТ обращаться напрямую к БД
class AJAX_Handler {
    public function handle() {
        $db = new NPK_Database(); // ❌ ЗАПРЕЩЕНО
        $db->get_fertilizers();   // ❌ ЗАПРЕЩЕНО
    }
}

// Shortcode НЕ МОЖЕТ вызывать Calculation_Engine
add_shortcode('npk', function() {
    $result = Calculation_Engine::calculate(); // ❌ ЗАПРЕЩЕНО
});
```

---

## 4. ИСКЛЮЧЕНИЯ (ТОЛЬКО ДЛЯ МОДУЛЯ 2)

| Компонент | Разрешено | Условия | Цитата |
|-----------|----------|---------|--------|
| **JavaScript (UI)** | ✅ **Алгоритм Отиаи для preview** | ТОЛЬКО Модуль 2, ТОЛЬКО предварительный просмотр, ЗАМЕНЯЕТСЯ ответом сервера | B.1 (исключение) |
| **JavaScript (UI)** | ✅ **Определение предварительного state** | По ТЕМ ЖЕ правилам, что и сервер, ЗАМЕНЯЕТСЯ финальным state | B.2, C.1 |
| **JavaScript (UI)** | ✅ **Упрощённый расчёт массы** | ТОЛЬКО для мгновенного UX, НЕ для финальных решений | D.1 |

**✅ ПРИМЕР РАЗРЕШЁННОГО ИСКЛЮЧЕНИЯ (ТОЛЬКО МОДУЛЬ 2):**
```javascript
// ✅ РАЗРЕШЕНО: Алгоритм Отиаи для preview
function previewMatchPercentage(cropNpk, fertilizerNpk) {
    // ТОЛЬКО для мгновенного отображения
    // БУДЕТ ЗАМЕНЕНО ответом сервера
    const normCrop = normalize(cropNpk);
    const normFert = normalize(fertilizerNpk);
    return Math.min(normCrop[0], normFert[0]) + 
           Math.min(normCrop[1], normFert[1]) + 
           Math.min(normCrop[2], normFert[2]) * 100;
}
```

---

## 5. ПРАВИЛО ДВУХУРОВНЕВОГО STATE (C.1)

| Что | Где определяется | Статус |
|-----|------------------|--------|
| **ФИНАЛЬНЫЙ state** | ✅ ТОЛЬКО в Calculation_Engine | ЕДИНСТВЕННЫЙ источник истины |
| **ПРЕДВАРИТЕЛЬНЫЙ state** | ✅ В JavaScript (для UX) | Заменяется финальным |

**❌ ЗАПРЕЩЕНО:**
```javascript
// ❌ НЕЛЬЗЯ: Игнорировать финальный state с сервера
function handleServerResponse(data) {
    if (data.state) {
        // ❌ Не показываем свой state, показываем серверный!
        updateUI(data.state);
    }
}

// ❌ НЕЛЬЗЯ: JS выбирает state по своим правилам
function getFinalState(concentration) {
    // ❌ Бизнес-логика ТОЛЬКО на сервере!
    if (concentration > 0.6) return 'danger';
}
```

---

## 6. КРИТЕРИЙ НЕПРИЁМКИ (F)

**Реализация НЕ ПРИНИМАЕТСЯ, ЕСЛИ:**

| Условие | Статус |
|---------|--------|
| `state` определяется вне `Calculation_Engine` | ❌ НЕПРИЁМКА |
| JavaScript содержит логику выбора `state` (кроме preview) | ❌ НЕПРИЁМКА |
| Data_Adapter участвует в расчётах | ❌ НЕПРИЁМКА |
| Итоговые значения могут отличаться без AJAX | ❌ НЕПРИЁМКА |
| В системе существует более одного «источника истины» | ❌ НЕПРИЁМКА |

---

## 7. ПРАВИЛО ДЛЯ ИСПОЛНИТЕЛЯ (G)

> **Если сомневаешься, где должна жить логика — ответ по умолчанию: `Calculation_Engine`**

| Если логика влияет на: | Место размещения |
|------------------------|------------------|
| ✅ Числа | `Calculation_Engine` |
| ✅ Состояние (state) | `Calculation_Engine` |
| ✅ Безопасность | `Calculation_Engine` |
| ✅ Интерпретацию данных | `Calculation_Engine` |
| ❌ UI/отображение | JavaScript/CSS |
| ❌ Транспорт | AJAX_Handler |
| ❌ Подготовку данных для UI | Data_Adapter |

---

## ✅ ИТОГ: ТРИ ГЛАВНЫХ ПРАВИЛА ДЛЯ ЧАТА 5

```
┌─────────────────────────────────────────────────────────────────────┐
│ 1. Calculation_Engine — ЕДИНСТВЕННЫЙ, КТО ИМЕЕТ ПРАВО СЧИТАТЬ    │
│    • ВСЕ формулы ТОЛЬКО здесь                                     │
│    • ВСЕ алгоритмы ТОЛЬКО здесь                                   │
│    • ВСЕ финальные решения ТОЛЬКО здесь                           │
│                                                                     │
│ 2. Data_Adapter — ТОЛЬКО ПОДГОТОВКА ДАННЫХ ДЛЯ UI                │
│    • Никаких расчётов                                             │
│    • Никакого участия в AJAX                                      │
│    • Никакого определения state                                   │
│                                                                     │
│ 3. JavaScript — ТОЛЬКО UX, НИКАКОЙ БИЗНЕС-ЛОГИКИ                 │
│    • Предварительный просмотр — ДА (только Модуль 2)             │
│    • Финальные решения — НЕТ                                     │
│    • Источник истины — НЕТ                                       │
└─────────────────────────────────────────────────────────────────────┘
```
  
  ],
  "контекст": "Модуль 2, файл: selector.js"
}
