---
tags:
  - ComfyUI
  - AI
Похожее:
  - Генерация изображений
Сфера:
  - "[[IT]]"
Источник:
  - DeepSeek
Связано с:
  - "[[ComfyUI]]"
Категория:
  - Конспект
Дата создания:
---
**Да, AI upscaler — это ноды в ComfyUI!** Их несколько видов, и они встроены или устанавливаются отдельно.

## 📦 **Типы апскейлер-нод в ComfyUI:**

### **1. Встроенные (есть по умолчанию):**
| Нода | Путь | Что делает |
|------|------|------------|
| **Image Scale** | `image/upscale image` | Простое увеличение (не AI) |
| **Image Scale By** | `image/upscale image` | Увеличение в N раз |
| **Latent Upscale** | `latent/upscale latent` | Увеличение в латентном пространстве |

### **2. Установочные (AI, рекомендуемые):**
| Нода | Пакет | Особенность |
|------|-------|-------------|
| **Ultimate SD Upscale** | `ComfyUI-Impact-Pack` | ⭐ Лучший! Работает с тайлингом |
| **Upscale Model Loader** + **Image Upscale with Model** | встроено | Использует ESRGAN модели |
| **Diffusion Upscaling** | через скрипты | Img2img-апскейл |

## 🎯 **Ultimate SD Upscale (самый популярный):**

### **Как установить:**
1. В ComfyUI Manager → Install Custom Nodes
2. Найдите **`ComfyUI-Impact-Pack`**
3. Установите и перезапустите

### **Как использовать:**
```
1. Load Image (ваше 1152x768)
2. Ultimate SD Upscale
3. Save Image (1200x800)
```

### **Настройки Ultimate SD Upscale:**
```
target_size: 1200x800
upscaler: 4x-UltraSharp
seed: -1 (случайный)
steps: 20-30
denoise: 0.2-0.35
tile_width: 512 (для экономии памяти)
```

## 🔧 **Другие AI апскейлеры:**

### **ESRGAN модели (скачать отдельно):**
```
Папка: ComfyUI/models/upscale_models/
Модели:
- 4x-UltraSharp.pth
- ESRGAN_4x.pth
- RealESRGAN_x4plus.pth
- Remacri.pth
```

### **Workflow с ESRGAN:**
```
Load Image → Upscale Model Loader → Image Upscale with Model → Save Image
```

## 🚀 **Полный workflow с апскейлом:**

### **Вариант A: Простой (без перегенерации)**
```
[Сгенерировали 1152x768]
    ↓
Load Image
    ↓
Ultimate SD Upscale (target: 1200x800)
    ↓
Save Image
```

### **Вариант B: С перегенерацией деталей**
```
Load Image (1152x768)
    ↓
Ultimate SD Upscale 
    ↓
VAE Encode (для img2img)
    ↓
KSampler (denoise: 0.25, steps: 15)
    ↓
VAE Decode
    ↓
Save Image (1200x800)
```

## 📊 **Сравнение методов:**

| Метод | Качество | Скорость | Память | Сложность |
|-------|----------|----------|---------|-----------|
| **Ultimate SD Upscale** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | Средняя |
| **ESRGAN** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐ | Простая |
| **Img2Img Upscale** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ | Сложная |
| **Простое увеличение** | ⭐ | ⭐⭐⭐⭐⭐ | ⭐ | Очень простая |

## 💡 **Для вашего случая (1152→1200):**

### **Самый простой способ:**
1. Установите **Impact Pack**
2. Используйте **Ultimate SD Upscale**
3. Настройки:
   ```
   target_width: 1200
   target_height: 800  
   upscaler: 4x-UltraSharp
   denoise: 0.2
   tile_width: 768
   ```

### **Если нет Impact Pack:**
```
1. Скачайте модель upscaler: "4x-UltraSharp.pth"
2. Положите в: ComfyUI/models/upscale_models/
3. Используйте ноды:
   - Upscale Model Loader (выберите 4x-UltraSharp)
   - Image Upscale with Model (scale_by: 1.04)
```

## 🌿 **Для растений оптимально:**

**Ultimate SD Upscale с настройками:**
```
model: epicrealismXL (та же, что для генерации)
seed: -1
steps: 20
denoise: 0.25
cfg: 7.0
tile_width: 640
```

## ⚠️ **Частые ошибки:**

1. **"Module not found: Impact Pack"** → не установлен пакет
2. **"No upscale models found"** → скачайте модели ESRGAN
3. **Артефакты на стыках тайлов** → уменьшите tile_width
4. **Слишком долго** → используйте denoise 0.15-0.2

## ✅ **Пошаговая инструкция:**

### **Шаг 1: Установка**
```
ComfyUI Manager → Install → ComfyUI-Impact-Pack
```

### **Шаг 2: Скачать модели апскейла**
Скачайте с Hugging Face:
- `4x-UltraSharp.pth`
- `RealESRGAN_x4plus.pth`

### **Шаг 3: Собрать workflow**
```
Load Image → Ultimate SD Upscale → Save Image
```

### **Шаг 4: Настройка**
```
target_size: 1200x800
upscaler: 4x-UltraSharp
denoise: 0.2-0.3
tile_width: 512-768
```

## 🆓 **Бесплатные альтернативы если не хочется ставить:**

1. **Внешний upscaler:** Upscayl (отдельная программа)
2. **Онлайн-сервисы:** iloveimg.com, letsenhance.io
3. **Фотошоп:** Super Resolution (если есть)

**Но Ultimate SD Upscale в ComfyUI — самый удобный,** так как всё в одном месте. Увеличение на 4% он сделает идеально!