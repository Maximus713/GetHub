# 🔧 **АНАЛИЗАТОР КОДА: ИНСТРУКЦИИ ПАТЧЕЙ ДЛЯ class-shortcode.php**

## 📋 **ПАТЧИ ДЛЯ: `module-concentration/includes/class-shortcode.php`**

### **⚠️ ВАЖНО:** Применяйте патчи в указанном порядке!

---

## **🔴 ПАТЧ 1: CRITICAL_001_SHORTCODE_CONSTANTS**
**Исправление неопределённых констант**

**НАЙТИ:**
```php
    private function render_template(string $calculator_id): string {
        // Проверяем существование файла шаблона
        if (!file_exists(NPK_TEMPLATE_PATH)) {
            return $this->get_fallback_html($calculator_id);
        }
```

**ЗАМЕНИТЬ НА:**
```php
    private function render_template(string $calculator_id): string {
        // Получаем путь к шаблону
        $template_path = plugin_dir_path(__FILE__) . '../templates/calculator-template.php';
        
        // Проверяем существование файла шаблона
        if (!file_exists($template_path)) {
            return $this->get_fallback_html($calculator_id);
        }
```

---

## **🔴 ПАТЧ 2: CRITICAL_002_SHORTCODE_DATA_TRANSFER**
**Упрощение передачи данных в JavaScript**

**НАЙТИ:**
```php
    private function localize_script(string $calculator_id, array $ui_data): void {
        // Создаём уникальное имя переменной для этого калькулятора
        $js_var_name = 'NPK_Concentration_Data_' . str_replace('-', '_', $calculator_id);
        
        wp_localize_script(
            'npk-concentration-calculator-js',
            $js_var_name,
            $ui_data
        );
```

**ЗАМЕНИТЬ НА:**
```php
    private function localize_script(string $calculator_id, array $ui_data): void {
        // Используем единое имя переменной для всех калькуляторов
        wp_localize_script(
            'npk-concentration-calculator-js',
            'NPK_Concentration_Data',
            $ui_data
        );
```

---

## **🔴 ПАТЧ 3: CRITICAL_003_SHORTCODE_DUPLICATE_DATA**
**Удаление дублирования данных**

**НАЙТИ:**
```php
        // Также добавляем глобальную переменную с ссылкой
        wp_add_inline_script(
            'npk-concentration-calculator-js',
            sprintf(
                'window.NPK_Concentration_Instances = window.NPK_Concentration_Instances || {};' . 
                'window.NPK_Concentration_Instances["%s"] = %s;',
                $calculator_id,
                $js_var_name
            )
        );
```

**ЗАМЕНИТЬ НА:**
```php
        // Больше не нужно дублировать данные - используем единую переменную
```

---

## **🟡 ПАТЧ 4: MAJOR_001_SHORTCODE_SCRIPT_CHECK**
**Добавление проверки зарегистрированных скриптов**

**НАЙТИ:**
```php
    private function enqueue_assets(string $calculator_id): void {
        // 1. Подключаем CSS стили
        wp_enqueue_style('npk-concentration-calculator-css');
        
        // 2. Подключаем JavaScript
        wp_enqueue_script('npk-concentration-calculator-js');
```

**ЗАМЕНИТЬ НА:**
```php
    private function enqueue_assets(string $calculator_id): void {
        // 1. Проверяем и подключаем CSS стили
        if (wp_style_is('npk-concentration-calculator-css', 'registered')) {
            wp_enqueue_style('npk-concentration-calculator-css');
        }
        
        // 2. Проверяем и подключаем JavaScript
        if (wp_script_is('npk-concentration-calculator-js', 'registered')) {
            wp_enqueue_script('npk-concentration-calculator-js');
        }
```

---

## **🟡 ПАТЧ 5: MAJOR_002_SHORTCODE_DATA_ADAPTER**
**Добавление обработки ошибок Data_Adapter**

**НАЙТИ:**
```php
    private function get_ui_data_for_calculator(string $calculator_id): array {
        // Проверяем существование Data_Adapter
        if (!class_exists('NPK_Concentration_Data_Adapter')) {
            return $this->get_fallback_ui_data($calculator_id);
        }
```

**ЗАМЕНИТЬ НА:**
```php
    private function get_ui_data_for_calculator(string $calculator_id): array {
        // Проверяем существование Data_Adapter и его работоспособность
        if (!class_exists('NPK_Concentration_Data_Adapter')) {
            error_log('NPK Shortcode: Data_Adapter class not found');
            return $this->get_fallback_ui_data($calculator_id);
        }
        
        try {
            // Получаем данные из Data_Adapter
            $adapter = NPK_Concentration_Data_Adapter::get_instance();
            $ui_data = $adapter->get_ui_data();
```

**ТАКЖЕ НАЙТИ КОНЕЦ ЭТОГО МЕТОДА (примерно строка 70):**
```php
        // Добавляем ID калькулятора
        $ui_data['calculator_id'] = $calculator_id;
        
        // Применяем фильтр WordPress для модификации данных
        $ui_data = apply_filters('npk_concentration_initial_data', $ui_data, $calculator_id);
        
        return $ui_data;
```

**ВСТАВИТЬ ПЕРЕД `return $ui_data;`:**
```php
        } catch (Exception $e) {
            error_log('NPK Shortcode: Error getting UI data: ' . $e->getMessage());
            return $this->get_fallback_ui_data($calculator_id);
        }
```

**ТАКЖЕ ИСПРАВИТЬ КОНЕЦ МЕТОДА (после добавления):**
```php
            // Добавляем ID калькулятора
            $ui_data['calculator_id'] = $calculator_id;
            
            // Применяем фильтр WordPress для модификации данных
            $ui_data = apply_filters('npk_concentration_initial_data', $ui_data, $calculator_id);
            
            return $ui_data;
        } catch (Exception $e) {
            error_log('NPK Shortcode: Error getting UI data: ' . $e->getMessage());
            return $this->get_fallback_ui_data($calculator_id);
        }
    }
```

---

## **🔵 ПАТЧ 6: MINOR_001_SHORTCODE_TEMPLATE_PATH**
**Исправление переменной в методе render_template**

**НАЙТИ (после применения патча 1):**
```php
        // Начинаем буферизацию вывода
        ob_start();
        
        // Включаем файл шаблона с передачей переменных
        include NPK_TEMPLATE_PATH;
        
        // Получаем содержимое буфера
        $html = ob_get_clean();
```

**ЗАМЕНИТЬ НА:**
```php
        // Начинаем буферизацию вывода
        ob_start();
        
        // Включаем файл шаблона с передачей переменных
        include $template_path;
        
        // Получаем содержимое буфера
        $html = ob_get_clean();
```

---

## ✅ **ПРОВЕРКА ПОСЛЕ ПАТЧЕЙ:**

### **СИНТАКСИЧЕСКАЯ ПРОВЕРКА:**
```bash
php -l module-concentration/includes/class-shortcode.php
```

### **ТЕСТОВЫЕ СЦЕНАРИИ:**

1. **Тест шорткода:**
   - Добавить `[npk_concentration_calculator]` на страницу
   - Проверить что калькулятор отображается без PHP ошибок

2. **Тест данных в JavaScript:**
   - Открыть консоль браузера
   - Проверить: `console.log(window.NPK_Concentration_Data)`
   - Должны быть данные удобрений, профилей и AJAX

3. **Тест AJAX запросов:**
   - Изменить параметры калькулятора
   - Проверить Network в DevTools
   - Запрос должен быть JSON с правильным Content-Type

4. **Тест без Data_Adapter:**
   - Временно переименовать класс Data_Adapter
   - Проверить что калькулятор всё равно отображается (с fallback данными)

---

## 📋 **СЛЕДУЮЩИЙ ШАГ:**

После успешного применения патчей к `class-shortcode.php`, нужно:

1. **Исправить `concentration-module.php`** для использования нового метода `register_hooks()`
2. **Протестировать интеграцию** всех компонентов
3. **Создать CSS файл** если его ещё нет

**❓ ВОПРОС:** Приступаем к анализу `concentration-module.php` для исправления инициализации?