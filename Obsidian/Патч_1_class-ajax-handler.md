# 🔧 **АНАЛИЗАТОР КОДА: ИНСТРУКЦИИ ПАТЧЕЙ**

## 📋 **ПАТЧИ ДЛЯ: `module-concentration/includes/class-ajax-handler.php`**

### **⚠️ ВАЖНО:** Применяйте патчи в указанном порядке!

---

## **🔴 ПАТЧ 1: CRITICAL_001_AJAX_FORMAT**
**Замена $_POST на JSON чтение**

**НАЙТИ:**
```php
        // 2. Без проверок, берём данные как есть
        $fertilizer_id = isset($_POST['fertilizer_id']) ? (int)$_POST['fertilizer_id'] : 1;
        $concentration = isset($_POST['concentration']) ? (float)$_POST['concentration'] : 0.3;
        $water_volume = isset($_POST['water_volume']) ? (float)$_POST['water_volume'] : 10.0;
        $ground_type = isset($_POST['ground_type']) ? sanitize_text_field($_POST['ground_type']) : 'wet';
```

**ЗАМЕНИТЬ НА:**
```php
        // 2. Получаем JSON данные из тела запроса
        $json_input = file_get_contents('php://input');
        $data = json_decode($json_input, true);
        
        if (!$data) {
            self::send_json_error('Некорректный JSON в запросе', 400);
            return;
        }
        
        // 3. Извлекаем и валидируем данные
        $fertilizer_id = isset($data['fertilizer_id']) ? (int)$data['fertilizer_id'] : 0;
        $concentration = isset($data['concentration']) ? (float)$data['concentration'] : 0.0;
        $water_volume = isset($data['water_volume']) ? (float)$data['water_volume'] : 0.0;
        $ground_type = isset($data['ground_type']) ? sanitize_text_field($data['ground_type']) : '';
```

---

## **🔴 ПАТЧ 2: CRITICAL_002_NONCE_SECURITY**
**Добавление проверки nonce**

**НАЙТИ:**
```php
    public static function handle_request(): void {
        // 0. Сразу отвечаем - для теста
        header('Content-Type: application/json');
```

**ЗАМЕНИТЬ НА:**
```php
    public static function handle_request(): void {
        // 0. Проверяем nonce для безопасности
        if (!self::verify_nonce()) {
            self::send_json_error('Ошибка безопасности: неверный nonce', 403);
            return;
        }
        
        // 1. Устанавливаем правильный Content-Type
        header('Content-Type: application/json');
```

**ДОБАВИТЬ НОВЫЕ МЕТОДЫ (в конец класса, перед последней `}`):**

```php
    /**
     * Проверка nonce
     * @return bool
     */
    private static function verify_nonce(): bool {
        $json_input = file_get_contents('php://input');
        $data = json_decode($json_input, true);
        
        $nonce = $data['nonce'] ?? '';
        
        if (empty($nonce)) {
            return false;
        }
        
        return wp_verify_nonce($nonce, 'npk_concentration_calculate');
    }
    
    /**
     * Отправка JSON ошибки
     * @param string $message Сообщение об ошибке
     * @param int $status_code HTTP статус код
     */
    private static function send_json_error(string $message, int $status_code = 400): void {
        status_header($status_code);
        wp_send_json([
            'success' => false,
            'data' => [
                'message' => $message
            ]
        ]);
    }
```

---

## **🔴 ПАТЧ 3: CRITICAL_003_VALIDATION_MISSING**
**Добавление валидации полей**

**НАЙТИ:**
```php
        error_log("Params: id={$fertilizer_id}, conc={$concentration}, vol={$water_volume}, ground={$ground_type}");
```

**ВСТАВИТЬ ПОСЛЕ ЭТОЙ СТРОКИ:**
```php
        
        // 4. Валидация обязательных полей согласно ТЗ п.7.3.2
        if (!self::validate_request_data($data)) {
            self::send_json_error('Некорректные данные запроса', 400);
            return;
        }
```

**ДОБАВИТЬ НОВЫЙ МЕТОД (после метода verify_nonce()):**

```php
    /**
     * Валидация данных запроса согласно ТЗ п.7.3.2
     * @param array $data Данные запроса
     * @return bool
     */
    private static function validate_request_data(array $data): bool {
        // Проверяем обязательные поля
        $required_fields = ['fertilizer_id', 'concentration', 'water_volume', 'ground_type'];
        
        foreach ($required_fields as $field) {
            if (!isset($data[$field])) {
                return false;
            }
        }
        
        // Проверяем типы данных
        if (!is_numeric($data['fertilizer_id']) || (int)$data['fertilizer_id'] <= 0) {
            return false;
        }
        
        if (!is_numeric($data['concentration']) || (float)$data['concentration'] <= 0) {
            return false;
        }
        
        if (!is_numeric($data['water_volume']) || (float)$data['water_volume'] <= 0) {
            return false;
        }
        
        // Проверяем допустимые значения ground_type
        $allowed_ground_types = ['dry', 'medium', 'wet'];
        if (!in_array($data['ground_type'], $allowed_ground_types)) {
            return false;
        }
        
        return true;
    }
```

---

## **🔴 ПАТЧ 4: CRITICAL_004_RESPONSE_FORMAT**
**Исправление формата ответа при ошибке**

**НАЙТИ:**
```php
        } catch (Exception $e) {
            error_log('Calculation ERROR: ' . $e->getMessage());
            
            echo json_encode([
                'success' => false,
                'message' => 'Ошибка расчёта: ' . $e->getMessage(),
                'data' => [
                    'fertilizer_mass' => 0,
                    'concentration' => 0,
                    'water_volume' => 0,
                    'state' => [
                        'id' => 'error',
                        'name' => 'Ошибка',
                        'color' => '#dc3545',
                        'emoji' => '❌',
                        'description' => 'Произошла ошибка при расчёте',
                        'range' => [0, 0]
                    ],
                    'warnings' => [$e->getMessage()]
                ]
            ]);
        }
```

**ЗАМЕНИТЬ НА:**
```php
        } catch (Exception $e) {
            error_log('Calculation ERROR: ' . $e->getMessage());
            self::send_json_error('Ошибка расчёта: ' . $e->getMessage(), 500);
        }
```

---

## **🟡 ПАТЧ 5: MAJOR_001_WORDPRESS_STANDARDS**
**Использование wp_send_json() вместо echo json_encode()**

**НАЙТИ:**
```php
            // 5. Возвращаем результат
            echo json_encode([
                'success' => true,
                'data' => $result
            ]);
```

**ЗАМЕНИТЬ НА:**
```php
            // 5. Возвращаем результат используя WordPress стандарты
            self::send_json_success($result);
```

**ДОБАВИТЬ НОВЫЙ МЕТОД (после метода send_json_error()):**

```php
    /**
     * Отправка успешного JSON ответа согласно ТЗ п.7.3.3
     * @param array $result_data Данные результата
     */
    private static function send_json_success(array $result_data): void {
        wp_send_json([
            'success' => true,
            'data' => $result_data
        ]);
    }
```

---

## **🟡 ПАТЧ 6: MAJOR_002_ERROR_LOG_PRODUCTION**
**Условное логирование только при WP_DEBUG**

**НАЙТИ:**
```php
        // 1. Просто логируем
        error_log('=== NPK AJAX CALLED === ' . date('Y-m-d H:i:s'));
        error_log('POST data: ' . json_encode($_POST));
```

**ЗАМЕНИТЬ НА:**
```php
        // 1. Отладочное логирование только при WP_DEBUG
        if (defined('WP_DEBUG') && WP_DEBUG) {
            error_log('=== NPK AJAX CALLED === ' . date('Y-m-d H:i:s'));
            $json_input = file_get_contents('php://input');
            error_log('Request data: ' . $json_input);
        }
```

---

## **🟡 ПАТЧ 7: MAJOR_003_REGISTRATION_HOOKS**
**Добавление метода регистрации AJAX хуков**

**НАЙТИ КОНЕЦ ФАЙЛА (последние строки):**
```php
        wp_die(); // Обязательно для AJAX в WordPress
    }
}
```

**ВСТАВИТЬ ПЕРЕД ПОСЛЕДНЕЙ `}`:**
```php
    
    /**
     * Регистрация AJAX хуков (должна вызываться из модуля)
     */
    public static function register_hooks(): void {
        add_action('wp_ajax_npk_concentration_calculate', [__CLASS__, 'handle_request']);
        add_action('wp_ajax_nopriv_npk_concentration_calculate', [__CLASS__, 'handle_request']);
    }
```

---

## **🔵 ПАТЧ 8: MINOR_001_CODE_QUALITY**
**Стандартизация выхода из функции**

**НАЙТИ:**
```php
        // 3. Проверяем что Calculation Engine загружен
        if (!class_exists('NPK_Concentration_Calculation_Engine')) {
            error_log('ERROR: Calculation Engine class not found');
            echo json_encode([
                'success' => false,
                'message' => 'Calculation Engine не загружен'
            ]);
            wp_die();
        }
```

**ЗАМЕНИТЬ НА:**
```php
        // 3. Проверяем что Calculation Engine загружен
        if (!class_exists('NPK_Concentration_Calculation_Engine')) {
            self::send_json_error('Calculation Engine не загружен', 500);
            return;
        }
```

---

## ✅ **ПРОВЕРКА ПОСЛЕ ПАТЧЕЙ:**

### **СИНТАКСИЧЕСКАЯ ПРОВЕРКА:**
```bash
php -l module-concentration/includes/class-ajax-handler.php
```

### **ТЕСТОВЫЕ СЦЕНАРИИ:**
1. **Тест на синтаксис:** Файл должен пройти без ошибок
2. **Тест на существование методов:** Убедитесь что добавлены:
   - `verify_nonce()`
   - `validate_request_data()`
   - `send_json_error()`
   - `send_json_success()`
   - `register_hooks()`
3. **Тест на соответствие ТЗ:** Формат запроса/ответа должен быть JSON

---

## 📋 **СЛЕДУЮЩИЙ ШАГ:**

После успешного применения патчей к `class-ajax-handler.php`, нужно:

1. **Обновить `calculator.js`** для работы с новым JSON форматом
2. **Обновить `concentration-module.php`** для использования нового `register_hooks()` метода

**❓ ВОПРОС:** Приступаем к анализу `calculator.js` для исправления AJAX вызовов?