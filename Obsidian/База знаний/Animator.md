#After-Effects 
[[Анимация текста]]

### Text / <span style='color:#ffcc99'>Animate</span>
Открывает доступ к [[Функции анимации текста]].
После выбора какой-либо функции Animate (например Position) появляется вкладка:
### <span style='color:#ffcc99'>Animator 1</span>
- Контейнер (их может быть несколько), в который помещена ВСЯ анимация. И дальнейшая анимация происходит не во вкладке "Transform", а во вкладках Animator 1, 2 и т.д.. Эту вкладку можно переименовать. В один аниматор можно добавлять несколько параметров из списка и их все можно будет анимировать через один параметр из вкладки Range Selector.

- Animator 1 (можно переименовать).
	-  <span style='color:#ffcc99'>Range Selector </span>- это своего рода "Эффектор", с помощью которого и будет происходить анимация:
		- "[[Start и End|Start]]" - Изменение с начала текста. (Упр. всеми функциями Animator)
		- "[[Start и End|End]]" - Изменение с конца текста. (Упр. всеми функциями Animator).
		- <span style='color:#ffcc99'>Offset</span> - сдвигает начальную точку действия анимации.
		- <span style='color:#ffcc99'>Advansed</span>
			- <span style='color:#ffcc99'>Units</span>- показывает значения "Start" и "End" в индексах или в %. Обычно используются %.
			- <span style='color:#ffcc99'>Based on</span> 
				-  <span style='color:#ffcc99'>Characters</span> - символы
				-  <span style='color:#ffcc99'>Characters excluding spaces</span> - символы, исключая пробелы
				-  <span style='color:#ffcc99'>Words</span> - слова
				-  <span style='color:#ffcc99'>Lines</span> - строки 
			- <span style='color:#ffcc99'>Mode</span> - Прием анимации
				- <span style='color:#ffcc99'>Subtract</span> - Меняет местами значения 0% и 100% параметра End
				- Intersect - ?
				- <span style='color:#ffcc99'>...</span>
			- <span style='color:#ffcc99'>Amount</span> - Сила воздействия от 0% до 100%. В какой-то степени дублирует опцию Start.
			- <span style='color:#ffcc99'>Shape</span> - Форма параметров анимации.
				- <span style='color:#ffcc99'>Square</span> - Анимация элемента начинается после завершения предыдущего.
				- <span style='color:#ffcc99'>Ramp Up</span> - Анимация "размытая", следующий элемент меняется не дожидаясь полного завершения предыдущего.
				- <span style='color:#ffcc99'>Ramp Down</span> - 
				- <span style='color:#ffcc99'>...</span>
			- <span style='color:#ffcc99'>Smoothness</span> - Плавность анимации. Обычно 100%.
			- <span style='color:#ffcc99'>Ease High</span> - Доп. параметры
			- <span style='color:#ffcc99'>Ease Low</span> - Доп. параметры.
			- <span style='color:#ffcc99'>Randomize Oreder</span> - Случайная, непоследовательная анимация.
				- <span style='color:#ffcc99'>Random Seed</span> - регулировка степени случайности.