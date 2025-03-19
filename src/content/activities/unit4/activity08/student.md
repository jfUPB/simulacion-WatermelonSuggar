**Simulación de una ola**

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/eDxcKvAkZ)

![image](https://github.com/user-attachments/assets/cae60a3d-4cab-4848-a53f-130c978416fa)


**Código**

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let angle = 10;
let angleVelocity = 0.08;
let amplitude = 60;

function setup() {
  createCanvas(640, 240);
  background(255);
  stroke(0);
  strokeWeight(2);
  fill(127, 127);
}

function draw() {
  background(255); // Vuelve a dibujar el fondo para eliminar las posiciones anteriores

  // Calcular la posición de los círculos y dibujarlos
  for (let x = 0; x <= width; x += 24) {
    // 1) Calcular la posición y en función de la amplitud y el seno del ángulo
    let y = amplitude * sin(angle + x * 0.02); // Desplazamiento adicional para que se vea la onda moverse
    // 2) Dibujar el círculo en la posición (x, y)
    circle(x, y + height / 2, 48);
  }

  // 3) Incrementar el ángulo para que la onda se mueva
  angle += angleVelocity;
}
```
