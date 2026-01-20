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

База данных npk-database.php для справки.
AJAX_Handler вызывает ТОЛЬКО Calculation_Engine::calculate().


# ФАЙЛ: `includes/class-ajax-handler.php`
# СЛОЙ: Транспорт
# НАЗНАЧЕНИЕ: Обработчик AJAX-запросов WordPress. Принимает данные от JS, передает их в Calculation_Engine, возвращает результат в формате JSON.

## 🎯 ОСНОВНАЯ ЦЕЛЬ ЭТОГО ФАЙЛА
Служит **мостом** между клиентским интерфейсом (JS) и ядром логики (`Calculation_Engine`). Отвечает за прием, валидацию и санитизацию входящих HTTP-параметров, вызов расчета и форматирование ответа (успех/ошибка) в JSON. Не содержит бизнес-логики.

## 📊 МЕСТО В АРХИТЕКТУРЕ
- **Находится в слое:** Транспорт (коммуникация Frontend-Backend)
- **Может вызывать:** Только `NPK_Concentration_Calculation_Engine::calculate()`.
- **Не может вызывать:** `NPK_Database`, `Data_Adapter`, `Shortcode`. Не выполняет расчёты самостоятельно.
- **Вызывается из:** JavaScript (`calculator.js`) через WordPress AJAX API (`wp_ajax_*` и `wp_ajax_nopriv_*`).

## ✅ РАЗРЕШЕНО (явный список)
1.  Регистрировать хуки WordPress AJAX (`wp_ajax_*`).
2.  Валидировать наличие, тип и диапазоны всех входящих параметров (`fertilizer_id`, `concentration`, `water_volume`, `ground_type`).
3.  Санитизировать (очищать) входящие строковые данные.
4.  Вызывать единственный метод `Calculation_Engine::calculate()` с подготовленными аргументами.
5.  Формировать структурированный JSON-ответ (как успешный, так и с ошибкой) согласно ТЗ п.7.3.
6.  Устанавливать корректные HTTP-статусы (200, 400, 500) и заголовки `Content-Type: application/json`.
7.  Логировать ошибки (например, через `error_log`) для отладки.

## ❌ ЗАПРЕЩЕНО (явный список, с акцентом на прошлые ошибки)
1.  **НИКАКОЙ бизнес-логики** (прошлая ошибка: чаты пытались здесь определять `state` или рассчитывать массу).
2.  **НИКАКИХ прямых обращений к `NPK_Database`** для проверок или получения данных (прошлая ошибка: дублирование логики `Calculation_Engine`).
3.  Участие в цепочке подготовки данных для UI (прошлая ошибка: совмещение с ролью `Data_Adapter`).
4.  Генерация HTML или модификация DOM.
5.  Отправка ответов в формате, отличном от JSON.
6.  Игнорирование валидации входных параметров.
7. 7. Любые обращения к `NPK_Database` (включая чтение констант).


## 📋 ОБЯЗАТЕЛЬНЫЕ МЕТОДЫ/ФУНКЦИИ
```php
<?php
/**
 * AJAX Handler для калькулятора концентрации.
 */
class NPK_Concentration_AJAX_Handler {

    /**
     * Регистрирует все AJAX-экшены модуля. Вызывается из концентрационного модуля.
     * @return void
     */
    public static function register_ajax_handlers(): void {
        add_action('wp_ajax_npk_concentration_calculate', [__CLASS__, 'handle_calculation']);
        add_action('wp_ajax_nopriv_npk_concentration_calculate', [__CLASS__, 'handle_calculation']);
    }

    /**
     * Основной обработчик AJAX-запроса на расчёт.
     * Собирает, валидирует параметры, вызывает Calculation_Engine, отправляет JSON.
     * @return void Завершает выполнение скрипта, отправляя JSON.
     */
    public static function handle_calculation(): void {
        // 1. Проверить nonce (если требуется безопасность).
        // 2. Получить и валидировать все 4 обязательных параметра из $_POST.
        // 3. Вызвать Calculation_Engine::calculate(...).
        // 4. Обработать возможное исключение из Calculation_Engine.
        // 5. Отправить JSON-ответ с соответствующим HTTP-кодом.
        wp_die(); // Обязательно завершить выполнение.
    }

    /**
     * Валидирует входные параметры запроса. Выбрасывает InvalidArgumentException при ошибке.
     * @param array $raw_post Данные $_POST.
     * @return array Ассоциативный массив с очищенными и приведёнными типами данными.
     * @throws InvalidArgumentException
     */
    protected static function validate_request_params(array $raw_post): array {
        // Проверяет наличие, тип, допустимые диапазоны для:
        
        // - fertilizer_id (int, >0). Проверка существования ID в базе данных выполняется **только** в `Calculation_Engine`.

        // - concentration (float, 0.05-1.5)
        // - water_volume (float, 0.5 - water_volume_max из констант)
        // - ground_type (string, в списке ['dry', 'medium', 'wet'])
        // Возвращает массив ['fertilizer_id' => int, ...].
    }

    /**
     * Формирует и отправляет JSON-ответ об успехе.
     * @param array $calculation_result Результат из Calculation_Engine::calculate().
     * @return void
     */
    protected static function send_success_response(array $calculation_result): void {
        wp_send_json_success(['data' => $calculation_result], 200);
    }

    /**
     * Формирует и отправляет JSON-ответ об ошибке.
     * @param string $message Человекочитаемое сообщение об ошибке.
     * @param int $http_code HTTP-статус код (400, 500).
     * @return void
     */
    protected static function send_error_response(string $message, int $http_code = 400): void {
        status_header($http_code);
        wp_send_json_error(['message' => $message], $http_code);
    }
}
```

## 🔗 ЗАВИСИМОСТИ И ДАННЫЕ
- **Получает данные из:** Глобального массива `$_POST` (параметры AJAX-запроса от JS).
- **Передает данные в:** Вызывает `NPK_Concentration_Calculation_Engine::calculate()` и отправляет JSON в ответ браузеру.

- **Использование констант:** AJAX-обработчик **НЕ имеет права** обращаться к `NPK_Database` напрямую. Проверка бизнес-ограничений (включая `water_volume_max`, допустимые диапазоны концентраций и существование `fertilizer_id`) является **исключительной ответственностью** `NPK_Concentration_Calculation_Engine`. AJAX выполняет только техническую валидацию (наличие параметров и корректность типов).

## ⚠️ КРИТИЧЕСКИЕ ТРЕБОВАНИЯ (нарушение = неприемка)
1.  **ТОЛЬКО ТРАНСПОРТ:** Файл является **чистым транспортом**. Любая логика, кроме валидации/санитизации входных данных и вызова `Calculation_Engine`, запрещена.
2.  **НАДЁЖНАЯ ВАЛИДАЦИЯ:** Должен отклонять любые запросы с некорректными, отсутствующими или выходящими за пределы данными с понятным сообщением об ошибке (HTTP 400).
3.  **ОБРАБОТКА ОШИБОК ЯДРА:** Должен корректно перехватывать любые исключения (например, `InvalidArgumentException` или другие) из `Calculation_Engine` и возвращать соответствующий JSON с HTTP 500 или 400.
4.  **СТАНДАРТНЫЙ ФОРМАТ ОТВЕТА:** Ответы ДОЛЖНЫ строго следовать форматам, описанным в ТЗ п.7.3.3 и 7.3.4 (`success:true/data` или `success:false/message`).

## 📝 ПРИМЕРЫ/ШАБЛОНЫ (если применимо)
```php
// Пример валидации ground_type в validate_request_params():
$allowed_ground_types = ['dry', 'medium', 'wet'];
if (!in_array($sanitized_ground_type, $allowed_ground_types, true)) {
    throw new InvalidArgumentException('Некорректный тип грунта.');
}

// Пример обработки в handle_calculation():
try {
    $params = self::validate_request_params($_POST);
    $result = NPK_Concentration_Calculation_Engine::calculate(
        $params['fertilizer_id'],
        $params['concentration'],
        $params['water_volume'],
        $params['ground_type']
    );
    self::send_success_response($result);
} catch (InvalidArgumentException $e) {
    self::send_error_response($e->getMessage(), 400);
} catch (Exception $e) {
    // Логируем неожиданную ошибку
    error_log('Calculation Engine Error: ' . $e->getMessage());
    self::send_error_response('Внутренняя ошибка сервера при расчёте.', 500);
}
```

---
