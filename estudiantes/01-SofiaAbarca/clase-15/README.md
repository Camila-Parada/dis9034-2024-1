# clase-15
Examen hecho en p5.js: v0.10.2

Para este examen quiero hacer algo que pueda usar para el Taller de Branding. En el taller estamos trabajando en una identidad visual basada en el concepto de "prueba y error", por lo que estoy pensando en crear algunas texturas usando los módulos que diseñamos y que pueda usar en las gráficas más adelante. 

Primero traje los módulos al sketch de p5, para darle a cada uno un movimiento distinto en el lienzo y que distorsionen una imagen de la faad.
Precargué las imágenes con la función preload

``` javascript

function preload() {
  
  faad = loadImage("faad.jpg");
  grafico = loadImage("./grafico.jpg");
  industrial = loadImage("./industrial.jpg");
  moda = loadImage("./moda.jpg");
}
```
Cada modulo tiene el nombre de una mención de diseño, para no confundirlos. Después hice el lienzo y puse de fondo la imagen de la faad, del mismo tamaño que el lienzo.
``` javascript
function setup() {
createCanvas(600, 600);
background(faad, 0, 0, width, height);
  
}
```
Encima de la foto de la faad quería que se fuera repitiendo el modulo llamado "gráfico". Pensé en usar lo que aprendimos sobre la aleatoriedad y el random() que tiene P5. Así que visité la referencia https://p5js.org/es/reference/#/p5/random y traté de replicar el tercer ejemplo, pero en vez de usar un punto, usar la imagen del módulo "gráfico". 

Para esto primero creé dos variables de posción x e y al principio del sketch.
``` javascript
let x = 200;
let y = 200;
```
Después dentro de draw() usé estas variables de posición junto con random(), para cambiar la posición de la foto. En este caso puse que la foto se dibujara entre -30 y 30 pixeles de distancia desde la posición original cada vez.
``` javascript
function draw() {
x += random(-30, 30);
y += random(-30, 30);
image(grafico, x, y, 50, 50);
```
Con esto logré que el modulo "gráfico" se dibujara de forma random encima de la foto de la faad, pero era muy rápido. Así que entré en la referencia de frameRate https://p5js.org/es/reference/#/p5/frameRate. Para hacerlo más lento tenía que darle un frame rate menor, así que lo dejé en 10 fotogramas por segundo, ya que el default son 60.
``` javascript
function setup() {
frameRate(10);
}
```

Tras esto pensé que quedaría bien ponerle algún filtro a la imagen para que se mezclara mejor con el fondo, y aplicar lo que vimos con los filtros https://p5js.org/es/reference/#/p5.Image/filter.  Los probé y ninguno me convenció, pero investigando las referencias encontré blendMode() https://p5js.org/es/reference/#/p5/blendMode y me gustaron más los efectos. Decidí ponerle a la imagen el modo exclusión.
``` javascript
blendMode(EXCLUSION);
```
Investigué en esta página que era lo que hacía exactamente los modos de fusión. https://helpx.adobe.com/cl/photoshop/using/blending-modes.html , ya que los conocía de photshop pero nunca lo había investigado más en profundidad.

Ahora quise hacer lo mismo con el módulo llamado "industrial" pero dandole un modo de fusión distinto. Hice otras dos variables de posición al principio del sketch y repetí el random() para darle aleatoriedad.
``` javascript
let a = 400;
let b = 400;
```
``` javascript
a += random(-30, 30);
  b += random(-30, 30);
      image(industrial, a, b, 80, 80);
```
Hasta aquí todo funcionaba bien, pero cuando le quise dar el modo de fusión blendMode(SOFT_LIGHT) no se aplicaba. Al parecer el primer modo de fusión afectaba a todo el sketch. Tuve que investigar si era posible tener más de un modo de fusión. Estrés. Tras buscar en las referencias encontré algo muy útil, push() y pop() https://p5js.org/es/reference/#/p5/push. También me vi el video de The Coding Train que explica como funcionaba esto https://www.youtube.com/watch?v=o9sgjuh-CBM.
Entonces lo probé en mi sketch. Hice un grupo con push() y pop () para ambas tranformaciones de módulos.
``` javascript
push()
blendMode(EXCLUSION);
x += random(-30, 30);
y += random(-30, 30);
image(grafico, x, y, 50, 50);
pop()
```
``` javascript
push();
blendMode(SOFT_LIGHT)
  a += random(-30, 30);
  b += random(-30, 30);
      image(industrial, a, b, 80, 80);
  pop();

```
Así logré tener dos modos de fusión distintos para cada una de las dos imágenes. Con esto solucionado repetí todo lo anterior con el tercer módulo llamado "moda", usando el modo overlay.
``` javascript
let c = 30;
let d = 30;
push();
  blendMode(OVERLAY);
  c += random(-30, 30);
  d += random(-30, 30);
      image(moda, c, d, 40, 40);
  pop();
```
Finalmente tocaba agregar algo de interactividad con el mouse. Encontré un ejemplo de p5 que ampliaba los pixeles al arrastrar el mouse, así que lo quise replicar en mi sketch siguiendo con el concepto. Volví a la referencia de mouseIsPressed https://p5js.org/es/reference/#/p5/mouseIsPressed. Hice dos constantes, para el largo y para el ancho.
``` javascript
if (mouseIsPressed) {
const randWidth = int(random(40, 50));
const randHeight = int(random(30, 80));
copy(mouseX, mouseY, 1, randHeight, mouseX, mouseY, randWidth, randHeight);
}
```
Es muy parecido a lo anterior, pero se activa mientras está presionado el mouse, selecciona los pixeles y gracias a copy() https://p5js.org/es/reference/#/p5.Image/copy, se copian los pixeles que están debajo del mouse y los repite hacia el lado y hacia abajo, usando las constantes anteriores.

Finalmente lo probé y funciona, pero a veces salta un error en la consola.
``` javascript
> p5.js says: copy() was expecting Integer for parameter #1 (zero-based index), received number instead at blob:https://preview.p5js.org/77c42ecf-36ea-4529-b563-32e626c539d7:92:5. [http://p5js.org/reference/#p5/copy]
```
Aún no lo puedo solucionar.

Al llegar a la U el código dejó de funcionar. Así que hablé con el profe y volví a la referencia de copy(). Nos dimos cuenta que faltaba el parámetro de source image, es decir, la imagen fuente que era copiada y dibujada de nuevo. Le puse de fuente la imagen "faad" y se solucionó el problema. :)
``` javascript
 copy(faad, mouseX, mouseY, 1, randHeight, mouseX, mouseY, randWidth, randHeight);
```

<img width="603" alt="Captura de pantalla 2024-06-24 a la(s) 15 23 49" src="https://github.com/SofiaAbarca/dis9034-2024-1/assets/163044808/fcfcf9a0-6751-461e-999c-355f462560f7">

Sketch del examen final: https://editor.p5js.org/SofiaAbarca/sketches/zn0A3Amwa


Otras cosas que probé/investigué en el proceso de hacer el examen:  https://editor.p5js.org/SofiaAbarca/sketches/Jq0uTJ95m , https://editor.p5js.org/SofiaAbarca/sketches/q4jZJ43bD , https://editor.p5js.org/SofiaAbarca/sketches/ZPv8fBjEU , 







 
