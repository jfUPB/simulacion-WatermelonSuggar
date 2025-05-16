**Simulación de istema de dos péndulos conectados en serie**

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/5V-BFQzNb)

![image](https://github.com/user-attachments/assets/30c4b895-f365-40c1-b8fe-2055d3702abe)

**Código**

**pendulum.js**

```js
class Pendulum {
  constructor(x, y, r, parent = null) {
    this.pivot = createVector(x, y);
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;

    this.angleVelocity = 0.0;
    this.angleAcceleration = 0.0;
    this.damping = 0.995;
    this.ballr = 24.0;
    this.dragging = false;
    this.parent = parent; // Si tiene un padre, significa que es el segundo péndulo
  }

  update() {
    if (!this.dragging) {
      let gravity = 0.4;
      this.angleAcceleration = ((-1 * gravity) / this.r) * sin(this.angle);

      this.angleVelocity += this.angleAcceleration;
      this.angle += this.angleVelocity;

      this.angleVelocity *= this.damping;
    }

    // Si el péndulo es el segundo, su pivote es el bob del primero
    if (this.parent) {
      this.pivot.set(this.parent.bob.x, this.parent.bob.y);
    }
  }
  show() {
    this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle), 0); //     Convertir coordenadas polares a cartesianas
    this.bob.add(this.pivot); // Ajustar la posición relativa

    stroke(0);
    strokeWeight(2);

    // Dibuja la línea primero para que quede detrás de la bola
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);

    fill(127);
    noStroke();
    // Dibuja la bola después para que quede sobre la línea
    circle(this.bob.x, this.bob.y, this.ballr * 2);
  }


  clicked(mx, my) {
    let d = dist(mx, my, this.bob.x, this.bob.y);
    if (d < this.ballr) {
      this.dragging = true;
    }
  }

  stopDragging() {
    this.angleVelocity = 0;
    this.dragging = false;
  }

  drag() {
    if (this.dragging) {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY));
      this.angle = atan2(-1 * diff.y, diff.x) - radians(90);
    }
  }
}
```

**sketch,js**

```js
let pendulum1, pendulum2;

function setup() {
  createCanvas(640, 400);
  
  // Crear dos péndulos conectados en serie
  pendulum1 = new Pendulum(width / 2, 10, 150);
  pendulum2 = new Pendulum(pendulum1.bob.x, pendulum1.bob.y, 150, pendulum1);
}

function draw() {
  background(255);

  // Actualizar y mostrar ambos péndulos
  pendulum1.update();
  pendulum1.show();
  pendulum1.drag();

  pendulum2.update();
  pendulum2.show();
  pendulum2.drag();

  // Verificar colisión y ajustar velocidades
  checkCollision();
}

function mousePressed() {
  pendulum1.clicked(mouseX, mouseY);
  pendulum2.clicked(mouseX, mouseY);
}

function mouseReleased() {
  pendulum1.stopDragging();
  pendulum2.stopDragging();
}

// Detecta si las esferas colisionan y ajusta velocidades para imitar un rebote
function checkCollision() {
  let d = dist(pendulum1.bob.x, pendulum1.bob.y, pendulum2.bob.x, pendulum2.bob.y);
  let minDist = pendulum1.ballr + pendulum2.ballr;

  if (d < minDist) {
    // Intercambiar velocidades para simular transferencia de energía
    let tempVel = pendulum1.angleVelocity;
    pendulum1.angleVelocity = pendulum2.angleVelocity;
    pendulum2.angleVelocity = tempVel;
  }
}
```
