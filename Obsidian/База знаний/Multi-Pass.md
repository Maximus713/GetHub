#Cinema_4D 
_______
Multi-Pass в Cinema 4D — это процесс разделения изображения на отдельные слои (потоки). [2](https://www.jigsawcad.com/How-to-render-pass-in-cinema-4d.html) 
Он позволяет изолировать аспекты общего изображения, включая тени, отражения, глубину и даже свойства отдельных материалов. [1](https://www.schoolofmotion.com/blog/export-multiple-passes-in-cinema-4d)

Multi-Pass используется для корректировки цвета и интенсивности освещения. [2](https://www.jigsawcad.com/How-to-render-pass-in-cinema-4d.html)
Очень полезны в пост обработке.
### Виды Multi-Pass:
- **Ambient Occlusion (AO)** — это модель затенения, (не заменяет, а дополняет поток теней от Cinema 4D) используемая в трёхмерной графике и позволяющая добавить реалистичности изображению за счёт вычисления интенсивности света, доходящего до точки поверхности. [4](https://otvet.mail.ru/question/222252756)

AO — это черно-белая текстура, в которой хранятся тени от рассеянного света. Они могут подчеркнуть рельеф, изображённый на основной текстуре. [2](https://3dclub.com/blog/kak-ispolzovat-karty-ao)

AO используется для достижения различных эффектов, таких как создание дополнительной темноты или контраста в затенённых областях. [3](https://help.maxon.net/c4d/s26/en-us/Content/_REDSHIFT_/html/Ambient%2BOcclusion.html)


- RGBA - это 32-битное цветовое пространство формата sRGB с альфа-каналом в Cinema 4D. Оно используется для добавления изображений к материалам. [1](https://docs.otoy.com/cinema4d/ImageTexture.html)
Изображения могут быть RGB, Alpha или монохромными. [1](https://docs.otoy.com/cinema4d/ImageTexture.html)

### Потоки Multi-Pass
- Background - 
- Alpha - Альфа канал для создания маски (чато)
- RGBA Image - (часто)
- Illumination - Свечение и тени
- Diffuse - размытие (часто)
- Specular - блики (часто)
- Shadow - Тени (часто)
- Ambient Occlusion - Затенение (часто)
- Global Illumination - (часто)
- Reflection - Отражение (часто)
- Refraction - Преломление (часто)
- Caustics - Каустика
- Post Effects - Все, что относится к коррекции цвета и др. (часто)
- Depth - размытие (эффект разфокусировки)
-  Material color - цвет материалов (редко)
- Material Diffusion - Размытие материалов (редко)
- Material Luminance - Свечение материалов (редко)
- Material Transparency - Прозрачность (редко)
- Transparency Reflection - серая маска изображения (редко)
- Material Environment - Окружение (редко)
- Material Specular - ? (редко)
- Material Specular Color - цвет свечения материала (редко)
- Material Normal - ? (редко)
- Material UVW - развертка (?) (редко)

Режимы наложения выбираются по конкретной задаче и ситуации.


