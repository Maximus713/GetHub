---
tags:
  - Ledgreen_ru
  - Калькулятор
  - Концентрация
  - AI
Похожее:
Сфера:
  - "[[Бизнес]]"
  - "[[IT]]"
  - "[[Web]]"
Источник:
Ссылки:
  - "[[Калькулятор удобрений]]"
  - "[[Алгоритм работы с раздельными файлами]]"
Категория:
Дата:
---

# ФАЙЛ: `assets/js/calculator.js`
# СЛОЙ: Представление (Frontend Logic / UI)
## НАЗНАЧЕНИЕ: Обеспечение интерактивности интерфейса калькулятора. Управление DOM-элементами, обработка событий, отправка AJAX-запросов и отображение результатов (предварительных и финальных).

## 🎯 ОСНОВНАЯ ЦЕЛЬ ЭТОГО ФАЙЛА
Создать отзывчивый и плавный пользовательский интерфейс, который **отображает** данные и состояния, рассчитывает **предварительные значения для мгновенного UX**, общается с сервером через AJAX и **синхронно применяет финальные данные от Calculation_Engine**. Не содержит бизнес-логики определения финального состояния или массы.

## 📊 МЕСТО В АРХИТЕКТУРЕ
- **Находится в слое:** Представление (Frontend, UI Logic)
- **Может вызывать:** Глобальный объект `NPK_Concentration_Data` (данные инициализации), Fetch API для AJAX-запросов к `wp-admin/admin-ajax.php`, методы DOM API.
- **Не может вызывать:** Не имеет доступа к PHP-классам. Не должен дублировать логику `Calculation_Engine` для финальных расчетов.
- **Вызывается из:** Загружается браузером после регистрации через `Shortcode`. Инициализируется автоматически при загрузке DOM.

## ✅ РАЗРЕШЕНО (явный список)
1.  **Для UX:** Определять **предварительный** `state` и **предварительную** массу, используя переданные диапазоны (`state.range`) и упрощенную формулу (без учета растворимости или с приближением), строго по алгоритму п.4.3 ТЗ.
2.  Управлять DOM: обновлять значения полей, двигать слайдер, изменять стили (цвета, видимость).
3.  Реализовывать интерактивность: обработку кликов, ввода, перемещения слайдера.
4.  Выполнять AJAX-запросы к `AJAX_Handler` с debounce/throttle и обрабатывать ответы (успех/ошибка).
5.  Рассчитывать и применять интерполяцию цвета фона поля "Общая концентрация" внутри диапазона текущего `state`.
6.  Рассчитывать и отображать поле "Концентрация NPK" на основе общей концентрации и `npk_sum` выбранного удобрения.
7.  Синхронно обновлять все цветовые элементы интерфейса (метка, слайдер, блок результатов) на основе актуального `state` (предварительного или финального).
8.  Обрабатывать ошибки сети/сервера и переходить на fallback-режим с использованием последних валидных данных или предварительных расчетов.

## ❌ ЗАПРЕЩЕНО (явный список, с акцентом на прошлые ошибки)
1.  **Быть источником истины для финальных данных** (прошлая ошибка: `state` и масса определялись только в JS и игнорировались ответы сервера).
2.  **Содержать финальную формулу расчета массы с учетом `solubility_total`** (прошлая ошибка: дублирование логики `Calculation_Engine`). Использовать можно только упрощенную формулу для мгновенного отображения.
3.  **Изменять алгоритм определения `state`** (прошлая ошибка: произвольные проверки `if (conc >= range[0])` с другими правилами). Алгоритм должен в точности повторять логику `Calculation_Engine` из п.4.3 ТЗ, но для предварительных целей.
4.  Хранить дубликаты данных из БД (все справочные данные брать из `NPK_Concentration_Data`).
5.  Генерировать HTML-разметку калькулятора (разметка должна быть в шаблоне `calculator-template.php`).
6. Запрещено использовать предварительный state для финальных решений или сохранять его после AJAX-ответа.


## 📋 ОБЯЗАТЕЛЬНЫЕ МЕТОДЫ/ФУНКЦИИ
```javascript
/**
 * Основной класс/модуль для управления фронтендом калькулятора концентрации.
 */
class NPKConcentrationCalculator {

updateUIWithPreview(preview) {
    this.currentState.preview = preview;

    this.ui.resultMass.textContent = `${preview.mass} г`;
    this.ui.resultConcentration.textContent = `${preview.concentration.toFixed(2)} %`;
    this.ui.npkConcentrationDisplay.textContent = preview.npkConcentration.toFixed(2);

    this.updateStateDisplay(preview.state);
    this.updateColorScheme(preview.state);
}

updateStateDisplay(state) {
    this.ui.resultState.textContent = state.label;
    this.ui.resultState.dataset.state = state.key;
}

bindEvents() {
    this.ui.concentrationSlider.addEventListener('input', (e) => {
        const val = parseFloat(e.target.value);
        this.currentState.concentration = val;
        this.ui.concentrationInput.value = val.toFixed(2);

        const preview = this.calculatePreview();
        this.updateUIWithPreview(preview);

        clearTimeout(this._debounce);
        this._debounce = setTimeout(() => {
            this.fetchCalculationFromServer();
        }, 400);
    });
}
populateInitialData() {
    this.data.fertilizers.forEach(f => {
        const opt = document.createElement('option');
        opt.value = f.id;
        opt.textContent = f.name;
        this.ui.fertilizerSelect.appendChild(opt);
    });

    this.ui.fertilizerSelect.value = this.currentState.fertilizer_id;
}

interpolateColor(c1, c2, f) {
    const hex = c => c.replace('#','');
    const r = c => parseInt(hex(c).substring(0,2),16);
    const g = c => parseInt(hex(c).substring(2,4),16);
    const b = c => parseInt(hex(c).substring(4,6),16);

    const rr = Math.round(r(c1) + f * (r(c2) - r(c1)));
    const gg = Math.round(g(c1) + f * (g(c2) - g(c1)));
    const bb = Math.round(b(c1) + f * (b(c2) - b(c1)));

    return `rgb(${rr},${gg},${bb})`;
}

handleAjaxError() {
    // fallback: ничего не делаем, остаёмся на preview
}


    /**
     * Конструктор. Принимает ID корневого DOM-элемента и данные инициализации.
     * @param {string} containerId ID элемента-контейнера.
     * @param {Object} initData Данные из NPK_Concentration_Data.
     */
    constructor(containerId, initData) {
        this.container = document.getElementById(containerId);
        this.data = initData;
        this.currentState = {
            fertilizer_id: initData.defaults.fertilizer_id,
            ground_type: initData.defaults.ground_type,
            concentration: initData.defaults.concentration,
            water_volume: initData.defaults.water_volume,
            // Предварительные значения, рассчитанные локально
            preview: null,
            // Финальные значения с сервера (изначально null)
            final: null
        };
        this.ui = {}; // Ссылки на DOM-элементы
        this.abortController = null; // Для отмены AJAX-запросов
        this.init();
    }

    /**
     * Основная инициализация: находит DOM-элементы, навешивает обработчики, выполняет первый расчет.
     */
    init() {
        this.cacheDomElements();
        this.bindEvents();
        this.populateInitialData(); // Заполняет селект удобрений, кнопки грунта
        this.performFullUpdate(); // Первый запуск: предварительный расчет + AJAX
    }
performFullUpdate() {
    const preview = this.calculatePreview();
    this.updateUIWithPreview(preview);
    this.fetchCalculationFromServer();
}

    /**
     * Находит и сохраняет ссылки на все необходимые DOM-элементы.
     */
    cacheDomElements() {
        this.ui = {
            fertilizerSelect: this.container.querySelector('#npk-fertilizer-select'),
            groundTypeButtons: this.container.querySelectorAll('.ground-type-btn'),
            concentrationInput: this.container.querySelector('#npk-concentration-input'),
            concentrationSlider: this.container.querySelector('#npk-concentration-slider'),
            waterVolumeInput: this.container.querySelector('#npk-water-volume-input'),
            npkConcentrationDisplay: this.container.querySelector('#npk-npk-concentration'),
            resultMass: this.container.querySelector('#npk-result-mass'),
            resultConcentration: this.container.querySelector('#npk-result-conc'),
            resultState: this.container.querySelector('#npk-result-state'),
            sliderLabel: this.container.querySelector('#npk-slider-label'),
            sliderFill: this.container.querySelector('#npk-slider-fill')
        };
    }

    /**
     * Рассчитывает предварительные данные (state и массу) на основе текущих значений в UI.
     * Используется для мгновенной обратной связи.
     * @returns {Object} Объект с preview данными { state, mass, concentration, waterVolume }.
     */
    calculatePreview() {
        const profile = this.data.profiles[this.currentState.ground_type];
        const fert = this.data.fertilizers.find(f => f.id === this.currentState.fertilizer_id);
        const conc = this.currentState.concentration;
        const vol = this.currentState.water_volume;

        // 1. Определить предварительный state по алгоритму п.4.3 (можно вынести в отдельный метод).
        const previewState = this.determinePreviewState(conc, profile);

        // 2. Рассчитать предварительную массу (упрощенная формула, БЕЗ учета растворимости для скорости).
        const previewMass = Math.round((conc / 100) * vol * 1000); // Упрощенно!

        // 3. Рассчитать концентрацию NPK.
        const npkConc = conc * (fert.npk_sum / 100);

        return {
            state: previewState,
            mass: previewMass,
            concentration: conc,
            waterVolume: vol,
            npkConcentration: npkConc
        };
    }

    /**
     * Определяет предварительный state по концентрации и профилю. Алгоритм ДОЛЖЕН повторять серверный (п.4.3 ТЗ).
     * @param {number} concentration Текущая концентрация.
     * @param {Object} profile Профиль грунта (с массивом states).
     * @returns {Object} Объект state (из данных профиля).
     */
    determinePreviewState(concentration, profile) {
        const states = profile.states;
        // Точная реализация алгоритма из п.4.3 ТЗ.
        for (let i = 0; i < states.length; i++) {
            const state = states[i];
            const [min, max] = state.range;
            // Для всех, кроме последнего: min <= conc < max
            if (i < states.length - 1) {
                if (concentration >= min && concentration < max) {
                    return state;
                }
            } else {
                // Для последнего: min <= conc <= max
                if (concentration >= min && concentration <= max) {
                    return state;
                }
            }
        }
        // Если концентрация ниже первого диапазона — вернуть первое состояние.
        if (concentration < states[0].range[0]) return states[0];
        // Если выше последнего — вернуть последнее.
        return states[states.length - 1];
    }

    /**
     * Отправляет AJAX-запрос к серверу для получения точных данных от Calculation_Engine.
     * Использует debounce и отмену предыдущих запросов.
     */
    async fetchCalculationFromServer() {
        // Отмена предыдущего запроса, если он еще выполняется.
        if (this.abortController) {
            this.abortController.abort();
        }
        this.abortController = new AbortController();

        const formData = new FormData();
        formData.append('action', 'npk_concentration_calculate');
        formData.append('fertilizer_id', this.currentState.fertilizer_id);
        formData.append('concentration', this.currentState.concentration);
        formData.append('water_volume', this.currentState.water_volume);
        formData.append('ground_type', this.currentState.ground_type);

        try {
            const response = await fetch(this.data.ajax_url, {
                method: 'POST',
                body: formData,
                signal: this.abortController.signal
            });

            if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);

            const result = await response.json();

            if (result.success) {
                this.currentState.final = result.data;
                this.updateUIWithFinalData(); // Синхронно обновить ВЕСЬ интерфейс финальными данными
            } else {
                throw new Error(result.data?.message || 'Server returned an error');
            }
        } catch (error) {
            if (error.name === 'AbortError') return; // Запрос был отменен, это нормально.
            console.error('AJAX request failed:', error);
            this.handleAjaxError();
        } finally {
            this.abortController = null;
        }
    }

    /**
     * Обновляет весь интерфейс (поля, слайдер, цвета, результаты) на основе финальных данных с сервера.
     * Должен вызываться только после успешного AJAX-ответа.
     */
    updateUIWithFinalData() {
        const final = this.currentState.final;
        // 1. Обновить числовые результаты (масса, концентрация)
        this.ui.resultMass.textContent = `${final.fertilizer_mass} г`;
        this.ui.resultConcentration.textContent = `${final.concentration.toFixed(2)} %`;
        // 2. Обновить блок состояния (используя final.state)
        this.updateStateDisplay(final.state);
        // 3. Синхронно обновить цвета ВСЕХ элементов на основе final.state
        this.updateColorScheme(final.state);
        // 4. Привести поля ввода в соответствие с финальными (скорректированными) значениями, если они изменились.
        if (Math.abs(final.concentration - this.currentState.concentration) > 0.001) {
            this.currentState.concentration = final.concentration;
            this.ui.concentrationInput.value = final.concentration.toFixed(2);
            this.ui.concentrationSlider.value = final.concentration;
        }
        // ... аналогично для water_volume
    }

    /**
     * Синхронно обновляет цвета всех элементов интерфейса на основе переданного state.
     * @param {Object} stateObj Объект state (предварительный или финальный).
     */
    updateColorScheme(stateObj) {
        const color = stateObj.color;
        // 1. Метка над слайдером
        this.ui.sliderLabel.style.backgroundColor = color;
        // 2. Заливка слайдера (элемент позади ползунка)
        this.ui.sliderFill.style.backgroundColor = color;
        // 3. Плашка состояния в блоке результатов
        this.ui.resultState.style.backgroundColor = color;
        // 4. Интерполяция фона поля ввода концентрации (от белого к color)
        this.updateConcentrationInputBackground(color, stateObj.range);
    }

    /**
     * Обновляет фон поля ввода концентрации с интерполяцией.
     * @param {string} stateColor Цвет состояния в HEX.
     * @param {Array} range Диапазон [min, max] текущего состояния.
     */
    updateConcentrationInputBackground(stateColor, range) {
        const conc = this.currentState.concentration;
        // Расчет веса внутри диапазона (от 0 до 1)
        let weight = (conc - range[0]) / (range[1] - range[0]);
        weight = Math.max(0, Math.min(1, weight)); // Ограничить
        // Интерполяция RGB между белым (#FFFFFF) и stateColor
        const bgColor = this.interpolateColor('#FFFFFF', stateColor, weight);
        this.ui.concentrationInput.style.backgroundColor = bgColor;
    }

    /**
     * Интерполяция цвета между двумя HEX-значениями.
     * @param {string} color1 Начальный цвет (HEX).
     * @param {string} color2 Конечный цвет (HEX).
     * @param {number} factor Вес (0 = color1, 1 = color2).
     * @returns {string} Результирующий цвет в формате rgb().
     */

}
```

## 🔗 ЗАВИСИМОСТИ И ДАННЫЕ
- **Получает данные из:** Глобального объекта `NPK_Concentration_Data`, переданного через `wp_localize_script`.
- **Передает данные в:** Сервер через `admin-ajax.php` (экшен `npk_concentration_calculate`).
- **Использует константы/глобалы:** `window.ajaxurl` (определяется WordPress), `NPK_Concentration_Data`.

## ⚠️ КРИТИЧЕСКИЕ ТРЕБОВАНИЯ (нарушение = неприемка)
1.  **ПРИОРИТЕТ СЕРВЕРНЫХ ДАННЫХ:** При получении успешного AJAX-ответа **ВСЕ** отображаемые данные (числа, состояния, цвета) должны быть немедленно и синхронно заменены на финальные с сервера. Предварительные данные исчезают.
2.  **СИНХРОННОСТЬ ОБНОВЛЕНИЯ ЦВЕТОВ:** При любом изменении `state` (предварительного или финального) цвет метки, слайдера, поля ввода и блока состояния должен обновиться **одновременно**, без расхождений.
3.  **ОТСУТСТВИЕ БИЗНЕС-ЛОГИКИ:** Файл содержит **логику отображения и UX**, но не бизнес-логику. Ответы на вопросы "опасна ли эта концентрация?" или "сколько точно нужно грамм?" дает только сервер.
4.  **ОБРАБОТКА ОШИБОК:** При сбое AJAX интерфейс не должен "ломаться". Должен отображаться понятный стилизованный message, а расчеты должны продолжиться на основе последних валидных данных или предварительных значений.

## 📝 ПРИМЕРЫ/ШАБЛОНЫ
```javascript
// Пример обработчика изменения слайдера с debounce для AJAX:
this.ui.concentrationSlider.addEventListener('input', (e) => {
    const newVal = parseFloat(e.target.value);
    this.currentState.concentration = newVal;
    // Мгновенное обновление поля ввода и предварительного расчета
    this.ui.concentrationInput.value = newVal.toFixed(2);
    const preview = this.calculatePreview();
    this.updateUIWithPreview(preview); // Метод для предварительного обновления UI

    // Отложенный AJAX-запрос с debounce
    clearTimeout(this.concentrationDebounceTimer);
    this.concentrationDebounceTimer = setTimeout(() => {
        this.fetchCalculationFromServer();
    }, 500);
});

// Пример fallback при ошибке AJAX:
handleAjaxError() {
    // Показать временное уведомление для пользователя
    this.showToast('Не удалось обновить расчёт. Проверено соединение.', 'warning');
    // Интерфейс продолжает работать на предварительных данных (уже отображенных)
    // Через 5 секунд можно попробовать повторный запрос
    setTimeout(() => this.fetchCalculationFromServer(), 5000);
}
```

---
