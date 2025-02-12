**Nuevo código**

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/wb13FIKJOE)

```js
let t = 0; // Factor de interpolación
let dir = 1; // Dirección de la interpolación (1: ida, -1: vuelta)
let speed = 0.015; // Velocidad de la animación1
let base;
let scaleFactor;

function setup() {
    createCanvas(400, 400);
    base = createVector(width / 2, height / 2); // Base inicial en el centro
}

function draw() {
    background(220);
    
    scaleFactor = map(mouseX, 0, width, 5, 20); // Escala dinámica con el mouse
    
    let v0 = createVector(30, 0).mult(scaleFactor);
    let v1 = createVector(0, 30).mult(scaleFactor);
    let v2 = p5.Vector.lerp(v0, v1, 0.5);
    let v4 = p5.Vector.sub(v1, v0); // Vector verde que cierra el triángulo
   let v3 = p5.Vector.lerp(v1, v2, t)
  

let colorStart = color(255, 0, 255); // Morado
    let colorEnd = color(0, 255, 255); // Cian
    let currentColor = lerpColor(colorStart, colorEnd, t); // Interpolación de color

    // La base de los vectores sigue el mouse
    base.set(mouseX, mouseY);


    drawArrow(base, v0, 'red'); // Vector rojo
    drawArrow(base, v1, 'blue'); // Vector azul
    //drawArrow(base, v3, 'purple'); // Vector intermedio
    drawArrow(p5.Vector.add(base, v0), v4, 'green'); // Vector verde
  
   drawArrow(base, v3, currentColor); // Vector morado animado con cambio de color

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

**¿Cómo agregué los cambios y solucioné los problemas?**

* Cambiar las bases de las flechas al usando el mouse
  * Antes la base de los vectores estaba fija en el centro del lienzo.
    Esto significa que todas las flechas partían del mismo punto, sin importar la posición del cursor.

  Ahora en casa frame (draw()), se actualiza la posición del mouse:+

  ```js
  base.set(mouseX, mouseY);
  ```











