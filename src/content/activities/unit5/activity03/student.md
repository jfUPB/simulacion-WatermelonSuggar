## Diseño de la obra

### Idea inicial:

* Quería trabajar sobre el ejemplo de polimorfismo y herencia de la actividad anterior porque le veía potencial, así que pensé que una buena idea sería volver a intervenirlo con los siguientes cambios:
  
    * Voy a aplicar los siguientes conceptos: vectores, fuerzas, péndulo (funciones sinusoidales) y distribuciones.
    * El canvas será de 500*500
    * El nuevo concepto será el de una nube que emite partículas en forma de gotas de lluvia.
    * El péndulo tendrá forma de rayo y ademas de cambiar el color de las partículas que toca también cambiará su forma para que sean estrellas
    * La gestión de partículas en memoria seguirá igual.
    * El color de fondo variará con una distribución gausiana para que cambie sutilmente entre colores azules.
    * Quiero que también se puedan crear más partículas presionando una tecla.


### Experimentación con código:

* Versión 1: simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/CAgpXP8T9)

 ![image](https://github.com/user-attachments/assets/c4b97b24-5255-47b9-ad3c-938f7c3f940c)

 **Cambios para la próxima versión**

> * Me di cuenta que la idea del rayo no me convencía tanto y las partículas sí se estan transformando en estrellas pero no había tanto dinamismo así que pensé que podía dejar también la funcionalidad del cambio de color para que fueran las probabilidades las que determinen si se cambia de color o de forma.
> * Para la vista es muy cansado tener el fondo cambiando ligeramente con la distribución gausseana así que mejor me voy por un solo color.
> * Quiero que sea una lluvia de partículas que se generarán en la parte superior de la pantalla.

Debes utilizar los conceptos de herencia y polimorfismo que revisaste en la fase de investigación.
Debes utilizar al menos un concepto de cada una de las unidades anteriores: 4 conceptos.
Debes definir cómo vas a gestionar el tiempo de vida de las partículas y la memoria.
La obra debe ser interactiva en tiempo real. Puedes usar teclado, mouse, música, el micrófono, video, sensor o cualquier otro dispositivo de entrada.
Documenta el proceso de creación, incluyendo la idea inicial, bocetos, experimentación con el código y el resultado final.

### Resultado final

Enlace de la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/5O_9JaUUV)

![image](https://github.com/user-attachments/assets/8774d5e9-87f3-452d-8c7c-0de647862209)

**El código**

**confetti.js**

```js
class Confetti extends Particle {
  constructor(x, y) {
    super(x, y);
    this.star = false;
  }

  show() {
    rectMode(CENTER);
    stroke(0, this.lifespan);
    if (this.transformed) {
      if (this.star) {
        fill(255, 215, 0); // Amarillo estrella
        push();
        translate(this.position.x, this.position.y);
        rotate(frameCount / 50.0);
        this.drawStar(0, 0, 5, 12, 5);
        pop();
      } else {
        fill(255, 165, 0); // Naranja
        push();
        translate(this.position.x, this.position.y);
        rotate(map(this.position.x, 0, width, 0, TWO_PI * 2));
        square(0, 0, 12);
        pop();
      }
    } else {
      fill(220, 220, 255, this.lifespan);
      push();
      translate(this.position.x, this.position.y);
      rotate(map(this.position.x, 0, width, 0, TWO_PI * 2));
      square(0, 0, 12);
      pop();
    }
  }

  drawStar(x, y, radius1, radius2, npoints) {
    let angle = TWO_PI / npoints;
    let halfAngle = angle / 2.0;
    beginShape();
    for (let a = 0; a < TWO_PI; a += angle) {
      let sx = x + cos(a) * radius2;
      let sy = y + sin(a) * radius2;
      vertex(sx, sy);
      sx = x + cos(a + halfAngle) * radius1;
      sy = y + sin(a + halfAngle) * radius1;
      vertex(sx, sy);
    }
    endShape(CLOSE);
  }
}
```

**emitter.js**

```js
class Emitter {
  constructor() {
    this.particles = [];
  }

  addParticle() {
    let x = random(width);
    let y = random(0, 10);
    if (random(1) < 0.5) {
      this.particles.push(new Particle(x, y));
    } else {
      this.particles.push(new Confetti(x, y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }

  getParticles() {
    return this.particles;
  }
}
```
**particle.js**

```js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(1, 3));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
    this.size = 8;
    this.transformed = false;
  }

  run() {
    this.applyForce(createVector(0, 0.05));
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    fill(this.transformed ? color(255, 165, 0) : color(220, 220, 255, this.lifespan));
    circle(this.position.x, this.position.y, this.size);
  }

  isDead() {
    return this.lifespan < 0;
  }
}

```
**pendulum.js**

```js
class Pendulum {
  constructor(x, y, r) {
    this.pivot = createVector(x, y);
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;
    this.angleVelocity = 0;
    this.angleAcceleration = 0;
    this.damping = 0.995;
    this.ballr = 20;
    this.dragging = false;
  }

  update() {
    if (!this.dragging) {
      let gravity = 0.4;
      this.angleAcceleration = (-gravity / this.r) * sin(this.angle);
      this.angleVelocity += this.angleAcceleration;
      this.angle += this.angleVelocity;
      this.angleVelocity *= this.damping;
    }
  }

  show() {
   this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle));
    this.bob.add(this.pivot);

    stroke(255);
    strokeWeight(2);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);

    fill(254, 255, 16 );
    noStroke();
    circle(this.bob.x, this.bob.y, this.ballr * 2);
    fill(3, 23, 58 );
    circle(this.bob.x + 5, this.bob.y, this.ballr * 2); // Parte oscura de la luna
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
      this.angle = atan2(-diff.y, diff.x) - PI / 2;
    }
  }

  checkCollision(particles) {
    for (let p of particles) {
      let d = dist(this.bob.x, this.bob.y, p.position.x, p.position.y);
      if (d < this.ballr + p.size / 2) {
        p.transformed = true;
        if (p instanceof Confetti) {
          p.star = random() < 0.5;
        }
      }
    }
  }
}

```

**sketch.js**

```js
let emitter;
let pendulum;

function setup() {
  createCanvas(500, 500);
  emitter = new Emitter();
  pendulum = new Pendulum(width / 2, 100, 200);
}

function draw() {
  background(3, 23, 58 );

  // Mayor cantidad de partículas por frame
  for (let i = 0; i < 2; i++) {
    emitter.addParticle();
  }

  emitter.run();

  pendulum.update();
  pendulum.drag();
  pendulum.show();
  pendulum.checkCollision(emitter.getParticles());
}

function mousePressed() {
  pendulum.clicked(mouseX, mouseY);
}

function mouseReleased() {
  pendulum.stopDragging();

}
```


Qué concepto de cada unidad aplicaste, cómo lo aplicaste y por qué.
Una captura de pantalla con una imagen de tu obra.
