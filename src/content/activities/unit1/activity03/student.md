#### Caminatas aleatorias

- Describe el experimento que vas a realizar.
  - **Qué pasaría si** se crearan 2 walkers que inician en lados opuestos del lienzo de manera simultánea y son un poco más visibles que el original.
  - **Qué pasaría si** estos dos walkers tienen a reunirse en el centro del canvas
  - **Qué pasaría si** después de reunirse en el centro emprenden de nuevoo su camino aleatorio
  - **Qué pasaría si** todo lo anterioor ocurre con 8 walkers

- ¿Qué pregunta quieres responder con este experimento?
  -¿Cómo a partir de una base de código sencilla puedo crear un sistema de random walks?

- ¿Qué resultados esperas obtener?
- ¿Qué resultados obtuviste?

**Código modificado**
```js
let walker1, walker2, walker3, walker4, walker5, walker6, walker7, walker8;

let walker1Reversed = false;
let walker2Reversed = false;

function setup() {
  createCanvas(640, 640);
  walker1 = new Walker(100, 100, color(0, 134, 255));
  walker2 = new Walker(540, 540, color(255, 201,0));
  walker3 = new Walker(540, 100, color(255, 251, 0)); 
  walker4 = new Walker(100, 540, color(255, 0, 255));
  walker5 = new Walker(100, 350, color (0, 255, 572));
  walker6 = new Walker(540, 350, color (229,36,36));
  walker7 = new Walker(350, 100, color (255, 0, 193));
  walker8 = new Walker(350, 540, color (124,255,0));
  background(0);
}

function draw() {
  walker1.step();
  walker1.show();

  walker2.step();
  walker2.show();

  walker3.step();
  walker3.show();

  walker4.step();
  walker4.show();

  walker5.step();
  walker5.show();

  walker6.step();
  walker6.show();

  walker7.step();
  walker7.show();

  walker8.step();
  walker8.show();
}

class Walker {
  constructor(x, y, col) {
    this.x = x || width / 2;
    this.y = y || height / 2;
    this.color = col || color(0);
    this.reversed = false;
  }

  show() {
    stroke(this.color);
    strokeWeight(5);
    point(this.x, this.y);
  }

  step() {
    const centerX = width / 2;
    const centerY = height / 2;
    const threshold = 5; // Umbral de proximidad al centro

    // Detectar si el "walker" llega al centro
    if (!this.reversed && abs(this.x - centerX) <= threshold && abs(this.y - centerY) <= threshold) {
      this.reversed = true; // Cambiar a modo de movimiento opuesto
    }

    // Movimiento aleatorio opuesto después de llegar al centro
    if (this.reversed) {
      const choice = floor(random(4));
      if (choice === 0) {
        this.x += 2; // Movimiento más rápido
      } else if (choice === 1) {
        this.x -= 2;
      } else if (choice === 2) {
        this.y += 2;
      } else {
        this.y -= 2;
      }
    } else {
      // Movimiento hacia el centro con un pequeño desvío aleatorio
      if (this.x < centerX) this.x += floor(random(1, 3));
      else if (this.x > centerX) this.x -= floor(random(1, 3));

      if (this.y < centerY) this.y += floor(random(1, 3));
      else if (this.y > centerY) this.y -= floor(random(1, 3));
    }
  }
}


```

- ¿Qué aprendiste de este experimento?

**Entrega**: los asuntos que te pido documentar en el enunciado, el código con las modificaciones y 
una captura de pantalla que muestre el resultado del experimento.
