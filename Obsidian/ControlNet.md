---
tags:
  - LedGreenBox
  - Ledgreen_ru
Похожее:
Сфера:
  - "[[Бизнес]]"
Источник:
Ссылки:
  - "[[ComfyUI]]"
Категория:
Дата:
---

Ноды:
- ComfyUI Manager (очень рекомендуется) — для простой установки.
- ControlNet Preprocessors (например, `ComfyUI-ControlNet-Aux`) — для предобработки (извлечения карт).

 **Установка через ComfyUI Manager (самый простой способ):**
- Запустите ComfyUI.
- В браузере перейдите в `http://127.0.0.1:8188/`.
- Нажмите кнопку **«Manager»**.
- Во вкладке **«Install Custom Nodes»** найдите и установите:
    - `ComfyUI-ControlNet-Aux` (для предобработки)
    - При необходимости другие пакеты (например, `ComfyUI-Advanced-ControlNet` для большего выбора).
- Перезапустите ComfyUI.

**3. Базовая рабочая цепочка (workflow):**  
Самый распространённый сценарий — контроль на основе существующего изображения. Цепочка будет выглядеть так:

`Загрузка изображения` -> `Предобработчик` -> `Модель ControlNet` -> `Загрузчик основной модели` -> `КСамpler` -> `VAE` -> `Сохранение`

4.**Как собрать workflow в интерфейсе:**
- **Загрузите исходное изображение:** Используйте ноду `Load Image`.
- **Извлеките карту управления:** Подключите изображение к **предобработчику** (например, `Canny Edge Detection`, `OpenPose`, `Depth`, `Scribble`). Вы получите карту (edge, pose, depth) и, иногда, preview-изображение.
- **Подготовьте модель ControlNet:** Добавьте ноду `Apply ControlNet`.
- **Настройте связи:**
    - **К `positive` и `negative`** подключите выходы текстовых энкодеров (CLIP) из вашего `CLIP Text Encode`.
    - **К `control_net`** подключите ноду `Load ControlNet Model` (укажите путь к `.safetensors` файлу ControlNet, например, `control_v11p_sd15_canny.pth`).
    - **К `image`** подключите выход **карты** (не preview!) из предобработчика.
    - Выходы `LATENT` подключите к `latent` входу вашего `KSampler`.

- **Совместимость моделей:** Используйте модели ControlNet, совместимые с вашей основной текстовой моделью (например, для SD1.5 — ControlNet для SD1.5).
    
- **Вес (strength):** В ноде `Apply ControlNet` параметр `strength` (от 0.0 до 2.0) определяет, насколько сильно влияние. Начните с 1.0.
    
- **Начните с простых предобработчиков:** **Canny** (контуры) или **Scribble** (рисунок от руки) — самые понятные для старта.
    
- **Используйте готовые workflow:** Загрузите примеры (`Load` -> найдите файлы .json или .png с workflow). Многие делятся ими на ресурсах вроде CivitAI или Reddit. Проанализируйте, как там собрана логика.
    
- **Где брать модели ControlNet?** С официального репозитория на Hugging Face или с CivitAI. Поместите их в папку `ComfyUI/models/controlnet/`.



Отличная идея! Вот таблица, распределяющая ключевые ноды ComfyUI по этапам использования ControlNet с комментариями.

## Таблица нод для работы с ControlNet в ComfyUI

| Этап процесса | Ноды (название и путь) | Комментарий и особенности |
|--------------|------------------------|---------------------------|
| **1. Загрузка изображения** | **`Load Image`**<br>`image/load image` | Базовая нода для загрузки изображения с диска. Поддерживает drag & drop. |
| | **`Load Image from URL`** (custom node) | Загружает изображение по ссылке. Полезно для быстрого тестирования без сохранения файлов. |
| | **`Empty Latent Image`**<br>`latent/empty latent` | Создаёт пустой латентный тензор заданного размера. Используется, когда нужно начать "с чистого листа" с маской. |
| **2. Предобработчик** | **`Canny Edge Detector`**<br>`ControlNet Preprocessors/Line Art` | Классический детектор краёв. Отлично сохраняет контуры объектов. Параметры `low_threshold`, `high_threshold` контролируют детализацию. |
| | **`MiDaS Depth Map`**<br>`ControlNet Preprocessors/Depth` | Оценивает глубину сцены (близкие объекты ярче, дальние темнее). Отлично сохраняет композицию и перспективу. |
| | **`OpenPose Pose Estimator`**<br>`ControlNet Preprocessors/Pose` | Определяет позу человека (скелет). Незаменим для точного позиционирования фигур. |
| | **`Scribble (Preprocessor)`**<br>`ControlNet Preprocessors/Scribble` | Преобразует изображение в рисованную схему. Идеален для передачи свободных эскизов. |
| | **`Line Art (Anime)`**<br>`ControlNet Preprocessors/Line Art` | Специализированный детектор линий в стиле аниме. Даёт более стилизованные контуры. |
| | **`Tile/Blur (Preprocessor)`**<br>`ControlNet Preprocessors/Blur` | Размывает изображение. Используется с моделями ControlNet Tile для улучшения деталей без изменения композиции. |
| **3. Модель ControlNet** | **`Load ControlNet Model`**<br>`loaders/load controlnet model` | Загружает файл модели ControlNet (.safetensors, .pth). **Критически важно** выбрать модель, соответствующую типу предобработки (canny, depth и т.д.). |
| | **`Apply ControlNet`**<br>`conditioning/apply controlnet` | **Сердце процесса.** Применяет загруженную модель ControlNet к условиям (positive/negative). Имеет параметр `strength` (сила влияния от 0.0 до 2.0). |
| | **`Apply Advanced ControlNet`** (custom node) | Расширенная версия с отдельными настройками силы для начала и конца генерации, возможностью применять несколько ControlNet. |
| **4. Загрузчик основной модели** | **`Checkpoint Loader`**<br>`loaders/checkpoint loader` | Загружает основную диффузионную модель (Stable Diffusion). Должна быть совместима с моделью ControlNet (обычно обе для SD1.5 или SDXL). |
| | **`Checkpoint Loader Simple`**<br>`loaders/checkpoint loader simple` | Упрощённая версия без предварительного просмотра. |
| **5. Кодирование текста** | **`CLIP Text Encode`**<br>`conditioning/clip text encode` | Кодирует текстовые промпты (positive/negative) в эмбендинги с помощью модели CLIP. Его выходы подключаются **до** `Apply ControlNet`. |
| **6. Сэмплер** | **`KSampler`**<br>`sampling/ksampler` | Основной сэмплер для диффузии. При использовании ControlNet, вход `latent` подключается **от выхода `Apply ControlNet`**, а не напрямую от CLIP. |
| | **`KSampler Advanced`**<br>`sampling/ksampler advanced` | Даёт более тонкий контроль над этапами сэмплинга (например, разные настройки для разных участков процесса). |
| **7. Декодирование (VAE)** | **`VAE Decode`**<br>`latent/vae decode` | Преобразует латентное представление обратно в пиксельное изображение. Использует VAE из загруженной основной модели. |
| | **`VAE Loader`**<br>`loaders/vae loader` | Позволяет загрузить отдельный, сторонний VAE (например, для улучшения детализации или цветопередачи). |
| **8. Сохранение** | **`Save Image`**<br>`image/save image` | Сохраняет изображение на диск. Можно задать папку и префикс имени. |
| | **`Preview Image`**<br>`image/preview image` | Показывает изображение в интерфейсе без сохранения на диск. Полезно для быстрого просмотра. |

---

## 🎯 **Ключевые моменты подключения (наглядно)**

### Стандартная цепочка БЕЗ ControlNet:
```
Checkpoint Loader -> CLIP Text Encode -> KSampler -> VAE Decode -> Save Image
                    (positive/negative)          ↑
                                            Empty Latent Image
```

### Цепочка С ControlNet:
```
Checkpoint Loader -> CLIP Text Encode ───┐
                                         ├──> Apply ControlNet -> KSampler -> VAE Decode -> Save Image
Load Image -> Canny Edge Detector ───┐   │                                    ↑
                                     ↓   ↓                                Empty Latent Image
Load ControlNet Model (canny)───────>ControlNet
```

### 🔧 **Специальные ноды для сложных сценариев**

| Нода | Назначение | Комментарий |
|------|------------|-------------|
| **`ControlNet Loader Advanced`** (из `ComfyUI-Advanced-ControlNet`) | Загрузка нескольких моделей ControlNet одновременно | Экономит место в workflow при использовании нескольких ControlNet. |
| **`IPAdapter`** (отдельный custom node) | Внедрение стиля или композиции из reference-изображения | Может работать в паре с ControlNet для комбинированного контроля. |
| **`Latent Composite`** | Наложение масок и комбинирование латентных представлений | Полезно для локальных правок после применения ControlNet. |
| **`Impact Switch`** (из `Impact Pack`) | Условное выполнение веток workflow | Позволяет создавать сложные логические цепочки (если есть depth, то применить depth ControlNet). |

## 💡 **Практический совет для начинающих**

Соберите сначала простую рабочую цепочку с **Canny Edge**:
1. `Load Image` → `Canny Edge Detector`
2. `Checkpoint Loader` → два `CLIP Text Encode` (positive/negative)
3. `Load ControlNet Model` (выберите `control_v11p_sd15_canny.pth`)
4. `Apply ControlNet` → подключите к нему ВСЕ три компонента выше
5. `Empty Latent Image` → `KSampler` (latent вход от Apply ControlNet)
6. `VAE Decode` → `Save Image`

Когда это заработает — экспериментируйте с другими типами предобработчиков и не забывайте менять модель ControlNet соответственно!



Список самых удачных и популярных моделей controlnet SDXL