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


Проверен


ЗАДАНИЕ ПРИНЯТО. Начинаю выполнение...

// ============================================
// ДЕБАГ-ПРОВЕРКИ: module-fertilizer-selector/assets/css/selector.css
// ============================================
// ЗАДАНИЕ №8: selector.css — Централизованные стили
//
// 1. Все пункты задания выполнены: ✅
//    - Все статические стили только здесь: ✅
//    - Все селекторы с префиксом .npk-: ✅
//    - 0 глобальных селекторов: ✅
//    - Стили прогресс-баров созданы: ✅ (.npk-progress-green, -orange, -grey)
//    - Стили для двух полей концентрации (B.5): ✅
//
// 2. Запреты соблюдены: ✅
//    - Глобальные селекторы (div, table, button): 0 результатов
//    - Инлайн-стили в JS/HTML: все статические стили здесь
//
// 3. Чек-лист самопроверки: ✅
//    - 0 селекторов без префикса .npk-: ✅
//    - Классы цветов прогресс-баров: ✅
//    - Адаптивность (медиа-запросы): ✅
//    - КТ-5: 0 глобальных селекторов: ✅
// ============================================
```css
/* ============================================
   NPK Fertilizer Selector - Centralized Styles
   Модуль 2: Подбор удобрений (Selector)
   Все статические стили ТОЛЬКО здесь.
   Все селекторы с префиксом .npk-
   ============================================ */

/* --------------------
   Основной контейнер
   -------------------- */
.npk-selector-container {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    max-width: 100%;
    margin: 1.5rem 0;
    padding: 1.25rem;
    background: #fff;
    border: 1px solid #e2e8f0;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.02);
}

/* --------------------
   Выпадающие списки
   -------------------- */
.npk-selector-dropdown {
    width: 100%;
    padding: 0.6rem 0.8rem;
    margin-bottom: 1.25rem;
    border: 1px solid #cbd5e0;
    border-radius: 6px;
    background-color: #fff;
    font-size: 1rem;
    line-height: 1.5;
    color: #2d3748;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.npk-selector-dropdown:focus {
    border-color: #4299e1;
    outline: 0;
    box-shadow: 0 0 0 3px rgba(66, 153, 225, 0.1);
}

.npk-selector-dropdown:disabled {
    background-color: #edf2f7;
    cursor: not-allowed;
    opacity: 0.6;
}

/* --------------------
   Результаты подбора (сетка карточек)
   -------------------- */
.npk-results-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.25rem;
    margin-top: 1.5rem;
}

/* --------------------
   Карточка удобрения
   -------------------- */
.npk-fertilizer-card {
    display: flex;
    flex-direction: column;
    padding: 1.25rem;
    background: #f9fafc;
    border: 1px solid #e9edf2;
    border-radius: 8px;
    transition: border-color 0.2s;
}

.npk-fertilizer-card:hover {
    border-color: #cbd5e0;
    background: #ffffff;
}

.npk-fertilizer-name {
    margin: 0 0 0.75rem 0;
    font-size: 1.1rem;
    font-weight: 600;
    color: #1a202c;
}

.npk-fertilizer-badge {
    display: inline-block;
    padding: 0.25rem 0.75rem;
    margin-bottom: 1rem;
    font-size: 0.85rem;
    font-weight: 500;
    color: #2b6f9b;
    background: #e6f0fa;
    border-radius: 20px;
    align-self: flex-start;
}

.npk-fertilizer-badge-fallback {
    background: #fff3cd;
    color: #856404;
}

/* --------------------
   Формула NPK и прогресс-бары
   -------------------- */
.npk-formula {
    font-size: 0.95rem;
    color: #4a5568;
    margin-bottom: 1rem;
    padding-bottom: 0.75rem;
    border-bottom: 1px dashed #e2e8f0;
}

.npk-progress-item {
    margin-bottom: 0.8rem;
}

.npk-progress-label {
    display: flex;
    justify-content: space-between;
    font-size: 0.85rem;
    margin-bottom: 0.2rem;
    color: #2d3748;
}

.npk-progress-bar-bg {
    height: 8px;
    background-color: #edf2f7;
    border-radius: 4px;
    overflow: hidden;
}

.npk-progress-bar-fill {
    height: 100%;
    width: 0%;
    transition: width 0.2s ease-in-out;
}

/* Цвета прогресс-баров (ТОЛЬКО классы, согласно ТЗ) */
.npk-progress-green {
    background: #28a745; /* >=85% */
}

.npk-progress-orange {
    background: #FFD700; /* 75-84% */
}

.npk-progress-grey {
    background: #6c757d; /* <75% */
}

/* --------------------
   Два поля концентрации (B.5)
   -------------------- */
.npk-concentration-row {
    display: flex;
    gap: 1rem;
    margin: 1rem 0 0.75rem 0;
    padding: 0.5rem 0;
    border-top: 1px solid #edf2f7;
}

.npk-concentration-field {
    flex: 1;
    display: flex;
    flex-direction: column;
}

.npk-concentration-label {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    color: #718096;
    margin-bottom: 0.2rem;
}

.npk-concentration-value {
    font-weight: 600;
    color: #2d3748;
}

/* --------------------
   Подсказки (tips)
   -------------------- */
.npk-tips-list {
    margin: 0.75rem 0 0 0;
    padding-left: 1.2rem;
    font-size: 0.85rem;
    color: #4a5568;
}

.npk-tip-item {
    margin-bottom: 0.25rem;
}

/* --------------------
   Состояние загрузки / ошибки
   -------------------- */
.npk-loading-indicator {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100px;
    color: #718096;
    font-style: italic;
}

.npk-error-message {
    padding: 1rem;
    margin: 1rem 0;
    background: #fee;
    border: 1px solid #fcc;
    border-radius: 6px;
    color: #a00;
}

/* --------------------
   Адаптивность (медиа-запросы)
   -------------------- */
@media (max-width: 768px) {
    .npk-selector-container {
        padding: 1rem;
    }

    .npk-results-grid {
        grid-template-columns: 1fr;
        gap: 1rem;
    }

    .npk-fertilizer-card {
        padding: 1rem;
    }

    .npk-concentration-row {
        flex-direction: column;
        gap: 0.5rem;
    }
}

@media (max-width: 480px) {
    .npk-selector-container {
        margin: 1rem 0;
        padding: 0.75rem;
        border-radius: 6px;
    }

    .npk-fertilizer-name {
        font-size: 1rem;
    }
}

/* --------------------
   Утилиты
   -------------------- */
.npk-hidden {
    display: none !important;
}

.npk-text-center {
    text-align: center;
}

.npk-mt-2 {
    margin-top: 0.5rem;
}

.npk-mb-2 {
    margin-bottom: 0.5rem;
}
```
// ============================================
// КОНЕЦ ФАЙЛА: module-fertilizer-selector/assets/css/selector.css
// ============================================