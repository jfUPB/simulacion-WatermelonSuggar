**Puntadas**

![image](https://github.com/user-attachments/assets/380b3653-e0d5-477c-a7c8-a9ee456ea39a)

Simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/xpA2RviYd)

**Código**

```js
let braids = [];

function setup() {
  createCanvas(600, 600);
  noFill();
  frameRate(30);
}

function draw() {
  background(20);

  if (mouseInCanvas()) {
    let gridSize = 50;
    let x = (mouseX - (mouseX % gridSize)) + gridSize / 2;
    let y = (mouseY - (mouseY % gridSize)) + gridSize / 2;
    braids.push(new Braid(x, y));
  }

  for (let braid of braids) {
    braid.update();
    braid.display();
  }
}

function mouseInCanvas() {
  return mouseX > 0 && mouseX < width && mouseY > 0 && mouseY < height;
}


class Braid {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.radius = 5;
    this.velocity = random(0.5, 2);
    this.acceleration = random(0.01, 0.05);
    this.maxRadius = random(20, 40);
    this.colors = [color(255, 0, 185), color(0, 139, 255), color(255, 166, 0)];
    this.angleOffset = random(TWO_PI);
  }

  update() {
    this.velocity += this.acceleration;
    this.radius += this.velocity;
    if (this.radius > this.maxRadius || this.radius < 5) {
      this.velocity *= -0.3; // Rebote al llegar al tamaño máximo o mínimo
    }
    this.angleOffset += 0.05; // Movimiento oscilante constante
  }

  display() {
    let numPoints = 100;
    let waveAmplitude = 10;
    for (let i = 0; i < 3; i++) {
      stroke(this.colors[i]);
      strokeWeight(4);
      noFill();
      beginShape();
      for (let j = 0; j < numPoints; j++) {
        let angle = j * 0.2 + i * PI / 3 + this.angleOffset;
        let r = this.radius + sin(j * 0.2 + this.angleOffset) * waveAmplitude;
        let x = this.x + r * cos(angle);
        let y = this.y + r * sin(angle);
        vertex(x, y);
      }
      endShape();
    }
  }
}



```


**Cambios aplicados:**

* El concepto varió un poquito (mucho :) ), sigue conservando la idea de crochet pero en lugar de crear las cadenas caracteríticas de los proyectos de tejido ahora hay punzadas circulares similares al tejido de anillos mágicos en los que se teje de manera circular.
  
* Los códigos de prueba para simular el tejido no salieron como esperaba porque eran aburridos y no simulaba realmente el tejido sino que eran figuras, así que cambié el enfoque para tratar de simular el tejido de una trenza y de allí surgieron las ideas de ondas sinusoidales que se transformaron en punzadas de color.
  
* En cuanto a la aplicación del Motion 101, cada puntada se crea en la posición del mouse, pero su crecimiento y forma se ven afectados por la velocidad y la aceleración, lo que genera una variación orgánica en el movimiento de cada "puntada".

  * Cada braid (punzada) se genera en una posición específica dentro de una cuadrícula, asegurando que los puntos sigan una estructura ordenada.
  * Cada punzada crece y se expande a una velocidad aleatoria entre 0.5 y 2, lo que hace que cada una tenga un ritmo único.
  * Cuando la punzada alcanza su tamaño máximo, la velocidad cambia de signo (* -0.3), haciendo que la forma "rebote" y oscile en tamaño.
  * En cada frame, la velocidad de crecimiento aumenta debido a una aceleración aleatoria entre 0.01 y 0.05.

* La interacción ya no depende de los clicks que pueda dar el usuario sino del movimiento del mouse sobre el canvas pues me resultaba más cómodo para evitar el pulso repetitivo y el molesto sonido del click después de un tiempo.
