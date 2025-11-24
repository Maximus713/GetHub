---
tags:
  - Stable_Diffusion
  - Промт
  - ComfyUI
  - Искусственный_интеллект
Похожее:
  - Запрос
Сфера:
  - "[[IT]]"
  - "[[Фото]]"
Источник:
Ссылки:
  - "[[Модели для stable-diffusion и ComfyUI]]"
  - "[[Параметры генерации]]"
  - "[[Создание персонажа]]"
  - "[[LORA]]"
Категория:
  - Конспект
Дата: 2025-11-21
---
[lexica.art](https://lexica.art/) Примеры работ с описаниями.
Добавить:
Список стилей 
Список художников

Общая стратегия создания изображения (спросить ИИ)


Порядок написания промта:
1. Что нужно создать (Photo, Digital artwok... ) В начале может идти усиление (ультра реалистичное и т.п.)
2. Описание объекта или персонажа.
3. Детали объекта.

Общая структура Промта.
[Тип изображения] + [Основной объект] + [Детализация объекта] + [Сцена и окружение] + [Композиция и ракурс] + [Освещение] + [Цветовая палитра и тон] + [Качество и детализация] + [Технические параметры]

Промты повышающие качество, реализм, свет, художественность и т.п.

### 2. Ключевые компоненты фотореалистичного промта

#### **A. Тип изображения (Жанр)**

Начните с самого главного. Это задает тон всей сцене.

- `photograph`, `photo`, `photorealistic`, `hyperrealistic`, `realistic photo`
    
- `35mm photograph`, `medium format photo`, `DSLR` (укажите тип камеры для определенного "глянца").
    
- `film still` (кадр из фильма), `documentary photography`, `street photography`, `portrait photography`, `product photography`.
    
> **Важно:** Использование `photograph` в начале промта часто более эффективно, чем `hyperrealistic`, так как последнее может иногда давать "перешарпленный" цифровой вид.

#### **B. Детализация объекта**

Чем больше конкретики, тем реальнее результат.

- **Для людей/животных:**
    
    - `highly detailed skin`, `skin pores`, `fine wrinkles`, `freckles`, `realistic eyes`, `wet eyes`, `detailed iris`, `natural skin texture`.
        
    - `sweat`, `water droplets`, `strands of hair`, `flyaway hair`.
        
- **Для объектов:**
    
    - `intricate details`, `textured surface`, `scratches`, `wear and tear`, `dust`, `reflections`.
        

#### **C. Освещение (Критически важный элемент!)**

Свет создает настроение и объем. Используйте конкретные термины.

- `soft light`, `hard light`, `rim light`, `backlight`, `golden hour lighting`, `blue hour lighting`.
    
- `cinematic lighting`, `studio lighting`, `dramatic lighting`, `natural lighting`, `sunlight`, `overcast light`.
    
- `light from a window`, `neon light`, `light and shadow`.
    

**Пример:** `...dramatic cinematic lighting with soft shadows and rim light...`

#### **D. Сцена и окружение**

Опишите контекст, в котором находится объект.

- `in a cozy cafe`, `on a rainy street`, `in a modern apartment`, `in a dense forest`, `at a bustling market`.
    
- `shallow depth of field`, `background blur` (размывает фон, акцентируя внимание на объекте).
    

#### **E. Композиция и ракурс**

Как кадр снят? Это влияет на динамику.

- `portrait`, `full body shot`, `close-up`, `extreme close-up`, `macro shot`.
    
- `low angle`, `high angle`, `eye level`.
    
- `rule of thirds`, `leading lines`, `dynamic angle`.
    

#### **F. Цветовая палитра и тон**

- `vibrant colors`, `muted colors`, `pastel colors`, `monochrome`, `black and white`.
    
- `warm tone`, `cool tone`, `cinematic color grading`, `film grain` (добавляет аналоговой текстуры).
    

#### **G. Качество и детализация (Чит-коды)**

Эти слова почти всегда должны быть в конце промта.

- `high detail`, `insanely detailed`, `ultra detailed`, `intricate details`.
    
- `8K`, `4K`, `UHD` (символизируют высочайшее качество).
    
- `sharp focus`, `professional color grading`, `masterpiece`, `best quality`.


### 2. Ключевые компоненты фотореалистичного промта

#### **A. Тип изображения (Жанр)**

- `photograph`, `photo` / **фотография**
    
- `photorealistic`, `realistic photo` / **фотореалистичный**, **реалистичная фотография**
    
- `hyperrealistic` / **гиперреалистичный**
    
- `35mm photograph`, `medium format photo` / **фотография с 35мм пленки**, **фотография с среднего формата**
    
- `DSLR` / **зеркальная камера**
    
- `film still` / **кадр из фильма**
    
- `documentary photography` / **документальная фотография**
    
- `street photography` / **уличная фотография**
    
- `portrait photography` / **портретная фотография**
    
- `product photography` / **предметная фотография**
    

#### **B. Детализация объекта**

- **Для людей/животных:**
    
    - `highly detailed skin`, `skin pores` / **высокодетализированная кожа**, **поры кожи**
        
    - `fine wrinkles` / **мелкие морщины**
        
    - `freckles` / **веснушки**
        
    - `realistic eyes`, `wet eyes`, `detailed iris` / **реалистичные глаза**, **влажные глаза**, **детализированная радужка**
        
    - `natural skin texture` / **естественная текстура кожи**
        
    - `sweat`, `water droplets` / **пот**, **капли воды**
        
    - `strands of hair`, `flyaway hair` / **пряди волос**, **отлетающие волоски**
        
- **Для объектов:**
    
    - `intricate details` / **сложные, замысловатые детали**
        
    - `textured surface` / **текстурированная поверхность**
        
    - `scratches` / **царапины**
        
    - `wear and tear` / **следы износа**
        
    - `dust` / **пыль**
        
    - `reflections` / **отражения**
        

#### **C. Освещение (Критически важный элемент!)**

- `soft light` / **мягкий свет**
    
- `hard light` / **жесткий свет**
    
- `rim light` / **контровой свет** (световой контур по краям объекта)
    
- `backlight` / **подсветка сзади**
    
- `golden hour lighting` / **свет "золотого часа"** (час после восхода или до заката)
    
- `blue hour lighting` / **свет "синего часа"** (сумерки)
    
- `cinematic lighting` / **кинематографическое освещение**
    
- `studio lighting` / **студийный свет**
    
- `dramatic lighting` / **драматический свет**
    
- `natural lighting`, `sunlight` / **естественное освещение**, **солнечный свет**
    
- `overcast light` / **свет в пасмурный день**
    
- `light from a window` / **свет из окна**
    
- `neon light` / **неоновый свет**
    
- `light and shadow` / **свет и тень**
    

#### **D. Сцена и окружение**

- `in a cozy cafe` / **в уютном кафе**
    
- `on a rainy street` / **на дождливой улице**
    
- `in a modern apartment` / **в современной квартире**
    
- `in a dense forest` / **в густом лесу**
    
- `at a bustling market` / **на шумном рынке**
    
- `shallow depth of field`, `background blur` / **малая глубина резкости**, **размытый фон**
    

#### **E. Композиция и ракурс**

- `portrait` / **портрет** (вертикальный кадр)
    
- `full body shot` / **кадр в полный рост**
    
- `close-up` / **крупный план**
    
- `extreme close-up` / **сверхкрупный план**
    
- `macro shot` / **макросъемка**
    
- `low angle` / **нижний ракурс** (съемка снизу)
    
- `high angle` / **верхний ракурс** (съемка сверху)
    
- `eye level` / **уровень глаз**
    
- `rule of thirds` / **правило третей**
    
- `leading lines` / **направляющие линии**
    
- `dynamic angle` / **динамичный угол**
    

#### **F. Цветовая палитра и тон**

- `vibrant colors` / **яркие, насыщенные цвета**
    
- `muted colors` / **приглушенные цвета**
    
- `pastel colors` / **пастельные тона**
    
- `monochrome` / **монохром**
    
- `black and white` / **черно-белое**
    
- `warm tone` / **теплый тон**
    
- `cool tone` / **холодный тон**
    
- `cinematic color grading` / **кинематографическая цветокоррекция**
    
- `film grain` / **зернистость пленки**
    

#### **G. Качество и детализация (Чит-коды)**

- `high detail` / **высокая детализация**
    
- `insanely detailed`, `ultra detailed` / **безумно детализировано**, **ультра детализировано**
    
- `intricate details` / **замысловатые детали**
    
- `8K`, `4K`, `UHD` / **(обозначения высокого разрешения)**
    
- `sharp focus` / **резкий фокус**
    
- `professional color grading` / **профессиональная цветокоррекция**
    
- `masterpiece` / **шедевр**
    
- `best quality` / **лучшее качество**

### 3. Анти-промт (Negative Prompt) — что исключить

**Стандартный анти-промт для фотореалистичности:**

text

cartoon, 3d, render, painting, drawing, anime, sketch, illustration, ugly, deformed, bad anatomy, disfigured, extra limbs, poorly drawn hands, poorly drawn face, mutation, mutated, blurry, noisy, jpeg artifacts, signature, watermark, username, text

**Перевод и смысл:**

- **`cartoon, 3d, render, painting, drawing, anime, sketch, illustration`** / **мультфильм, 3d, рендер, картина, рисунок, аниме, эскиз, иллюстрация** (исключают ненатуральные стили)
    
- **`ugly, deformed, bad anatomy, disfigured`** / **уродливый, деформированный, плохая анатомия, обезображенный**
    
- **`extra limbs, poorly drawn hands, poorly drawn face`** / **лишние конечности, плохо нарисованные руки, плохо нарисованное лицо** (борьба с частыми артефактами AI)
    
- **`mutation, mutated`** / **мутация, мутировавший**
    
- **`blurry, noisy`** / **размытый, шумный** (обеспечивает четкость)
    
- **`jpeg artifacts`** / **артефакты JPEG-сжатия**
    
- **`signature, watermark, username, text`** / **подпись, водяной знак, имя пользователя, текст** (убирает случайные надписи)
    

---

### 4. Пример промта с переводом

**Английская версия:**

> `A realistic photograph of an old sailor with a weathered face, deep wrinkles, and a worn beanie. He is on a fishing pier at dawn, with mist and seagulls in the background. Cinematic lighting with rim light, shallow depth of field. Highly detailed skin, realistic eyes, 8K, ultra detailed, sharp focus.`

**Русский перевод смысла:**

> **Реалистичная фотография** пожилого моряка с **обветренным лицом**, **глубокими морщинами** и в **поношенной вязаной шапке**. Он находится на **рыболовном пирсе на рассвете**, с **туманом** и **чайками** на **фоне**. **Кинематографическое освещение** с **контровым светом**, **малая глубина резкости**. **Высокодетализированная кожа**, **реалистичные глаза**, **8K**, **ультра детализировано**, **резкий фокус**.


masterpiece / максимальное качество