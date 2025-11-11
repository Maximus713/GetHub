#After-Effects 

Выражения (Expressions) - задают дополнительные изменения параметров анимации.
- Для включения: - Alt + параметр (циферблат) нужной анимации.

- loopOut("continue")
- loopOut("cycle")
- loopOut («pingpong»)
- wiggle (2,5)   - колебания

#### Отскок
amp = .1;
freq = 2.0;
decay = 2.0;
n = 0;
time_max = 4;
if (numKeys > 0){
n = nearestKey(time).index;
if (key(n).time > time){
n--;
}}
if (n == 0){ t = 0;
}else{
t = time - key(n).time;
}
if (n > 0 && t < time_max){
v = velocityAtTime(key(n).time - thisComp.frameDuration/10);
value + v*amp*Math.sin(freq*t*2*Math.PI)/Math.exp(decay*t);
}else{value}
