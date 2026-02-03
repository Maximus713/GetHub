## ЧТО УДАЛОСЬ ВЫЯСНИТЬ:

1. **CSS файл загружается** (статус 200 в Network)
2. **Элементы калькулятора есть в DOM** (102 элемента с классами `npk`)
3. **Кнопки грунта отображаются** (`display: flex`, `visibility: visible`)
4. **Используются классы ВТОРОЙ версии**:
   - `.npk-ground-button` (не `.npk-ground-btn`)
   - `.npk-ground-selection` (не `.npk-ground-type-panel`)
   - `.npk-ground-buttons` (не `.npk-ground-type-buttons`)

## ПРОБЛЕМА:
**CSS файл написан для классов ПЕРВОЙ (компактной) версии, а HTML использует классы ВТОРОЙ (широкой) версии.**

## ЧТО ДЕЛАТЬ ДАЛЬШЕ:

### Вариант 1: **Обновить CSS файл** (рекомендую)
В файле `npk-calculator-refactored.css`:
1. **Найти и заменить ВСЕ** старые классы на новые:
   - `.npk-ground-btn` → `.npk-ground-button`
   - `.npk-ground-type-panel` → `.npk-ground-selection`
   - `.npk-ground-type-buttons` → `.npk-ground-buttons`

2. **Или добавить в КОНЕЦ файла:**
```css
/* Исправление классов для второй версии */
.npk-ground-button {
    /* стили из .npk-ground-btn */
}

.npk-ground-selection {
    /* стили из .npk-ground-type-panel */
}

.npk-ground-buttons {
    display: flex !important;
    flex-direction: row !important;
    justify-content: center !important;
    gap: 12px !important;
    flex-wrap: wrap !important;
}
```

### Вариант 2: **Исправить HTML**
Вернуть в HTML второй версии старые классы (если хотите использовать существующий CSS).

### Вариант 3: **Проверить приоритет**
Убедиться, что CSS второй версии загружается ПОСЛЕ первой версии.

## САМОЕ ПРОСТОЕ:
**Добавьте в конец CSS файла исправления из п.1 (Вариант 1) и проверьте.**