**Simulación de resortes**

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/FplUZnuOG)

![image](https://github.com/user-attachments/assets/dea74283-5386-4451-b8d8-a2c3f1266a7a)

**Código**

**sketch.js**


```js
let bob;
let spring1;
let spring2;

function setup() {
  createCanvas(640, 240);
  // Modificamos las posiciones para que los resortes estén más separados
  spring1 = new Spring(width / 4, 90, 150);  // Resorte más largo
  spring2 = new Spring(3 * width / 4, 10, 150);  // Resorte más largo
  bob = new Bob(width / 2, 200);
}

function draw() {
  background(255);

  // Gravedad aplicada al bob
  let gravity = createVector(0, 0.2); // Menos gravedad para un movimiento más controlado
  bob.applyForce(gravity);

  // Actualizar bob y manejar el arrastre con el mouse
  bob.update();
  bob.handleDrag(mouseX, mouseY);

  // Cambiar la constante del resorte (elasticidad) dinámicamente
  let distance = dist(mouseX, mouseY, bob.position.x, bob.position.y);
  let elasticity = map(distance, 0, width / 2, 0.05, 0.5); // Valor de k más pequeño

  // Aplicar la fuerza de los resortes
  spring1.k = elasticity;
  spring2.k = elasticity;

  // Conectar los resortes al bob y restringir la longitud
  spring1.connect(bob);
  spring2.connect(bob);

  spring1.constrainLength(bob, 50, 250);  // Rango de longitud permitida mayor
  spring2.constrainLength(bob, 50, 250);  // Rango de longitud permitida mayor

  // Dibujar los resortes y el bob
  spring1.showLine(bob);
  spring2.showLine(bob);
  bob.show();
  spring1.show();
  spring2.show();
}

function mousePressed() {
  bob.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob.stopDragging();
}

```

**spring.js**

```js
class Spring {
  constructor(x, y, length) {
    this.anchor = createVector(x, y);
    this.restLength = length;
    this.k = 0.2; // Elasticidad inicial
  }

  // Calcular y aplicar la fuerza del resorte
  connect(bob) {
    let force = p5.Vector.sub(bob.position, this.anchor);
    let currentLength = force.mag();
    let stretch = currentLength - this.restLength;
    force.setMag(-1 * this.k * stretch);
    bob.applyForce(force);
  }

  constrainLength(bob, minlen, maxlen) {
    let direction = p5.Vector.sub(bob.position, this.anchor);
    let length = direction.mag();

    if (length < minlen) {
      direction.setMag(minlen);
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    } else if (length > maxlen) {
      direction.setMag(maxlen);
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    }
  }

  show() {
    fill(127);
    circle(this.anchor.x, this.anchor.y, 10);
  }

  showLine(bob) {
    stroke(0);
    line(bob.position.x, bob.position.y, this.anchor.x, this.anchor.y);
  }
}
```


