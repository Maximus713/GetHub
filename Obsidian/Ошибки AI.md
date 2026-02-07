---
tags:
  - LedGreenBox
  - Ledgreen_ru
Похожее:
Сфера:
  - "[[Бизнес]]"
Источник:
Ссылки:
  - "[[Ошибки шорт код]]"
  - "[[Калькулятор концентрации]]"
Категория:
Дата:
---
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

**НЕПРАВИЛЬНО (страница перезагружается):**
```javascript
// ❌ Форматы, которые вызывают перезагрузку:
new FormData()
new URLSearchParams()
'application/x-www-form-urlencoded'

fetch(url, {
    body: new FormData(form) // ❌ ПЕРЕЗАГРУЗКА!
})
```

**ПРАВИЛЬНО:**
```javascript
// ✅ ЕДИНСТВЕННЫЙ правильный формат:
fetch(ajaxurl, {
    method: 'POST',
    headers: { 
        'Content-Type': 'application/json' // ✅ ТОЛЬКО JSON
    },
    body: JSON.stringify({ // ✅ ТОЛЬКО JSON.stringify
        action: 'npk_concentration_calculate',
        fertilizer_id: 1,
        concentration: 0.3,
        water_volume: 10,
        ground_type: 'wet'
    })
})
```

**И в PHP тоже JSON:**
```php
// Получаем JSON
$json_input = file_get_contents('php://input');
$data = json_decode($json_input, true);

// Возвращаем JSON
wp_send_json_success($result);
```

**Признак ошибки:** Страница мигает и перезагружается при нажатии кнопок.

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





## 🎯 **ОБЯЗАТЕЛЬНЫЕ ПРАВИЛА**

### **ПРАВИЛО 1: СНАЧАЛА КЛАССЫ, ПОТОМ РЕГИСТРАЦИЯ**
```
1. Загрузить ВСЕ классы
2. Зарегистрировать шорткоды
3. Инициализировать модуль
```

### **ПРАВИЛО 2: JSON ДЛЯ AJAX**
```
ТОЛЬКО:
Content-Type: application/json
body: JSON.stringify()
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
4. [ ] **AJAX:** Страница не перезагружается при работе с калькулятором?

### **Проверь эти 3 пункта (качество):**
5. [ ] **UI:** Нет морганий при изменении значений? (`requestAnimationFrame`)
6. [ ] **Запросы:** Нет множественных AJAX при быстром вводе? (дебаунс)
7. [ ] **Отмена:** Предыдущие AJAX запросы отменяются? (`AbortController`)

---

**Этот список содержит ТОЛЬКО проверенные и важные ошибки.** Он короче, но точнее и полезнее для разработчиков.