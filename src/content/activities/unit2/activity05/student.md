**EL CÓDIGO**

```js
let t = 0; // Factor de interpolación
let dir = 1; // Dirección de la interpolación (1: ida, -1: vuelta)
let speed = 0.015; // Velocidad de la animación1

function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(255);

    let scaleFactor = 10; // Escala de los vectores

    let v0 = createVector(50, 50);
    let v1 = createVector(30, 0).mult(scaleFactor);
    let v2 = createVector(0, 30).mult(scaleFactor);
    let v4 = p5.Vector.sub(v2, v1); // Vector verde
    let v3 = p5.Vector.lerp(v1, v2, t); // Punto animado sobre el vector verde

    let colorStart = color(255, 0, 255); // Morado
    let colorEnd = color(0, 255, 255); // Cian
    let currentColor = lerpColor(colorStart, colorEnd, t); // Interpolación de color

    drawArrow(v0, v1, 'red'); // Vector rojo
    drawArrow(v0, v2, 'blue'); // Vector azul
    drawArrow(p5.Vector.add(v0, v1), v4, 'green'); // Vector verde
    drawArrow(v0, v3, currentColor); // Vector morado animado con cambio de color

    // Animación: modificar t de ida y vuelta
    t += speed * dir;
    if (t >= 1 || t <= 0) {
        dir *= -1; // Cambia de dirección
    }
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
**¿Cómo funciona lerp() y lerpColor().**

* lerp() -> Interpola linealmente dos vectores 

```js
let v3 = p5.Vector.lerp(v1, v2, t); // Punto animado sobre el vector verde
```
 v1: representa el punto de inicio del movimiento
 v2: indica el punto final del movimiento
 t: es un valor entre 0 y 1 que indica cuánto se ha movido entre los dos puntos 

Cuando t = 0, v3 es igual a v1 (inicio del vector verde).
Cuando t = 1, v3 es igual a v2 (final del vector verde).
Cuando t = 0.5, v3 es exactamente la mitad del camino entre v1 y v2.
Como t oscila entre 0 y 1, el vector morado se mueve de ida y vuelta sobre el vector verde.


* lerpColor()
  
```js
let currentColor = lerpColor(colorStart, colorEnd, t); // Interpolación de color
```

Cuando t = 0, currentColor es morado (colorStart).
Cuando t = 1, currentColor es cian (colorEnd).
Cuando t = 0.5, currentColor es una mezcla entre ambos (azulado-lila).
Como t oscila entre 0 y 1, el color cambia de ida y vuelta entre morado y cian.


**¿Cómo se dibuja una flecha usando drawArrow()?**

Para dibujar una flecha usando la función drawArrow(), debes proporcionar tres parámetros:

**base:** La posición inicial de la flecha (un p5.Vector).

**vec:** La dirección y magnitud del vector (también un p5.Vector).

**myColor:** El color de la flecha.


```js
function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor); // Color de la línea
    strokeWeight(3); // Grosor de la línea
    fill(myColor); // Color de la punta de la flecha
    translate(base.x, base.y); // Mueve el origen al punto base
    line(0, 0, vec.x, vec.y); // Dibuja la línea de la flecha
    rotate(vec.heading()); // Rota el sistema para apuntar en la dirección del vector
    let arrowSize = 7; // Tamaño de la punta de la flecha
    translate(vec.mag() - arrowSize, 0); // Posiciona la punta al final de la línea
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0); // Dibuja la punta
    pop();
}
```
> Cuando invocamos la función para crear un nuevo vector es cuando le pasamos los parámetros para que pueda realizar las operaciones correspondientes.
