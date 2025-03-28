**Simulación del problema de los n-cuerpos**

![image](https://github.com/user-attachments/assets/5cee4214-d8b4-44fa-8258-30dbc2c804f1)


Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/y-dCTDp7T)

```js
let bodies = [];
const G = 1;
const numBodies = 10;

function setup() {
  createCanvas(600, 600);
  for (let i = 0; i < numBodies; i++) {
    let x = randomGaussian(width / 2, width / 6);
    let y = randomGaussian(height / 2, height / 6);
    let mass = abs(randomGaussian(10, 5));
    let vx = randomGaussian(0, 1);
    let vy = randomGaussian(0, 1);
    bodies.push(new Body(x, y, vx, vy, mass));
  }
}

function draw() {
  background(220);
  for (let body of bodies) {
    body.update(bodies);
    body.display();
  }
}

class Body {
  constructor(x, y, vx, vy, mass) {
    this.position = createVector(x, y);
    this.velocity = createVector(vx, vy);
    this.acceleration = createVector(0, 0);
    this.mass = mass;

    // Aplicamos distribución normal para el color
    this.color = color(
      randomGaussian(50, 50), // Rojo (media 50, desviación estándar 50)
      randomGaussian(100, 50), // Verde (media 100, desviación estándar 50)
      randomGaussian(200, 50)  // Azul (media 200, desviación estándar 50)
    );
    this.color = constrainColor(this.color); // Aseguramos que los valores de color estén dentro de los límites
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update(bodies) {
    for (let other of bodies) {
      if (other !== this) {
        let force = this.calculateAttraction(other);
        this.applyForce(force);
      }
    }

    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);

    // Rebotar las partículas en los bordes del canvas
    this.checkEdges();
  }

  calculateAttraction(other) {
    let force = p5.Vector.sub(other.position, this.position);
    let distance = constrain(force.mag(), 5, 50);
    let strength = (G * this.mass * other.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  checkEdges() {
    // Rebotar al llegar a los bordes del canvas
    if (this.position.x < 0 || this.position.x > width) {
      this.velocity.x *= -1;
    }
    if (this.position.y < 0 || this.position.y > height) {
      this.velocity.y *= -1;
    }
  }

  display() {
    fill(this.color);
    noStroke();
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
}

// Función para garantizar que los valores del color estén dentro del rango válido [0, 255]
function constrainColor(c) {
  return color(
    constrain(red(c), 0, 255),
    constrain(green(c), 0, 255),
    constrain(blue(c), 0, 255)
  );
}

```

* El problema de los n-cuerpos trata sobre cómo varios cuerpos interactúan entre sí debido a la gravedad. En el código, este problema se modela mediante partículas que se atraen gravitacionalmente. 
Cada partícula tiene propiedades como posición, velocidad, masa y aceleración. La fuerza gravitacional entre dos partículas se calcula usando la ley de gravitación de Newton, que depende de sus masas y la distancia entre ellas.
En cada ciclo de la simulación, se calculan las fuerzas gravitacionales entre todas las partículas. Estas fuerzas actualizan la aceleración, velocidad y posición de cada partícula, lo que hace que se muevan en el espacio. 
Además, se simula el rebote de las partículas en los bordes del lienzo, evitando que se salgan de la pantalla.
Este modelo simplificado simula cómo las partículas interactúan y se mueven bajo la gravedad, ofreciendo una versión básica del problema de los n-cuerpos en 2D.
