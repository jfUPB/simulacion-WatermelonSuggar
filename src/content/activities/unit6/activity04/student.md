## Aplicación creativa de Flow Fields

### El concepto
* La idea es representar un ciberataque a sistemas de datos. El usuario es el hacker "infecta" un sistema, las partículas son los expertos en ciberseguridad que intentan detener al atacante que parece ser inofensivo, sin embargo, él ya ha tomado el control del sistema y los elimina uno a uno hasta apoderarse de todo.

### Adaptación

**Zonas de glitch que simulan la afectación del virus**

```js
let isGlitchZone = dist(mouseX, mouseY, x * scl, y * scl) < 100;
let angle = glitchMode || isGlitchZone ? random(TWO_PI) : noise(xoff, yoff) * TWO_PI * 4;
```

- Si estás en glitchMode o el mouse está cerca de una zona, el ángulo es aleatorio.
- Esto rompe el patrón orgánico del flowfield y genera movimientos erráticos → estética glitch.

______________________________________________________________________________________________

**La escala o resolución se adapta al campo**

```js
scl = glitchMode ? 300 : 200;
```

- En modo glitch se hace menos denso (menos filas/Columnas)
- Esto cambia la estructura del campo y el comportamiento de las partículas.

**Partículas influenciadas por el campo**

```js
particle.follow(flowField);
```
- Cada partícula consulta la fuerza del flowfield local y se mueve con base en ella.
- El applyForce() aplica esta dirección suavemente.

**Atracción hacia el cursor con oscilación**

Aunque esto no es del flow field en sí, complementa el movimiento con una fuerza que se superpone a la del campo:

```js
let forceX = (mouseX - this.pos.x) * attractionStrength + angleOffsetX;
let forceY = (mouseY - this.pos.y) * attractionStrength + angleOffsetY;
```

- Esto hace que parezca que el “virus” (cursor) está atrayendo o manipulando las partículas.
- Tiene un toque “hipnótico” gracias a los sin y cos.

### Interacción implementada 

* **Al hacer click se activa el modo "glitch":**
  
  * Cambia el estado del sistema (glitchMode).
  * Se recalcula la grilla (calculateGrid()).
  * Se genera una explosión en la posición del cursor.

* **Atracción hacia el mouse**
  
  * Las partículas se sienten atraídas hacia el mouse, con una fuerza que disminuye con la distancia.
  * Se suma un movimiento oscilante para hacerlo más expresivo.
 
* **Reinicio del sistema con barra espaciadora**
  * Al presionar espacio, se reinician las partículas y las explosiones.

* **Explosiones que afectan a las partículas**
  * Aplica una fuerza repulsiva a las partículas cercanas.
  * "Mata" partículas si están demasiado cerca.
 
### El código 

Simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/mOb1ie3Gd)

```js
let particles = [];
let flowField;
let cols, rows;
let scl = 200;
let inc = 0.1;
let explosions = [];
let glitchMode = false;

function setup() {
  createCanvas(windowWidth, windowHeight);
  calculateGrid();
  flowField = new Array(cols * rows);

  for (let i = 0; i < 4000; i++) {
    particles.push(new Particle());
  }

  background(0);
}

function draw() {
  background(0, 10);

  // Flow field
  let yoff = 0;
  for (let y = 0; y < rows; y++) {
    let xoff = 0;
    for (let x = 0; x < cols; x++) {
      let index = x + y * cols;
      let isGlitchZone = dist(mouseX, mouseY, x * scl, y * scl) < 100;
      let angle = glitchMode || isGlitchZone ? random(TWO_PI) : noise(xoff, yoff) * TWO_PI * 4;
      let v = p5.Vector.fromAngle(angle);
      v.setMag(1);
      flowField[index] = v;
      xoff += inc;
    }
    yoff += inc;
  }

  // Explosiones
  for (let explosion of explosions) {
    explosion.update();
    explosion.show();
  }

  // Actualizar partículas
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];

    for (let explosion of explosions) {
      explosion.affect(particle);
      if (explosion.kills(particle)) {
        particles.splice(i, 1);
        break;
      }
    }

    if (particles[i]) {
      particle.follow(flowField);
      particle.update();
      particle.edges();
      particle.show();
    }
  }

  explosions = explosions.filter(e => !e.isDone());

  noFill();
  stroke(0, 255, 255, 50);
  strokeWeight(2);
  ellipse(mouseX, mouseY, sin(frameCount * 0.1) * 50 + 100);
}

function mousePressed() {
  glitchMode = !glitchMode;
  calculateGrid();
  explosions.push(new Explosion(mouseX, mouseY));
}

// Evento de teclado para reiniciar el sistema
function keyPressed() {
  if (key === 'r' || key === 'R') {
    restart();
  }
}

function keyPressed() {
  if (keyCode === 32) { // 32 es el código de la barra espaciadora
    restart();
  }
}
function restart() {
  particles = [];
  explosions = [];
  calculateGrid();
  for (let i = 0; i < 4000; i++) {
    particles.push(new Particle());
  }
  background(0);
}

function calculateGrid() {
  scl = glitchMode ? 300 : 200;
  cols = floor(width / scl);
  rows = floor(height / scl);
  flowField = new Array(cols * rows);
}

class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxSpeed = glitchMode ? 5 : 2;
    this.prevPos = this.pos.copy();
    this.attractionFactor = random(0.2, 1);
  }

  follow(vectors) {
    let x = floor(this.pos.x / scl);
    let y = floor(this.pos.y / scl);
    let index = x + y * cols;
    let force = vectors[index];
    if (force) this.applyForce(force);
  }

  applyForce(force) {
    let maxForce = glitchMode ? 0.5 : 0.2;
    this.acc.add(p5.Vector.mult(force, maxForce));
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  edges() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }

  show() {
    let distToMouse = dist(this.pos.x, this.pos.y, mouseX, mouseY);
    if (random(1) < this.attractionFactor) {
      let attractionStrength = map(distToMouse, 0, width, 0.05, 0);
      let angleOffsetX = sin(frameCount * 0.1) * 2;
      let angleOffsetY = cos(frameCount * 0.1) * 2;
      let forceX = (mouseX - this.pos.x) * attractionStrength + angleOffsetX;
      let forceY = (mouseY - this.pos.y) * attractionStrength + angleOffsetY;
      this.vel.x += forceX;
      this.vel.y += forceY;
    }

    let colorChange = map(distToMouse, 0, width, 255, 0);
    stroke(colorChange, 100, 255 - colorChange, 100);
    strokeWeight(1);
    line(this.pos.x, this.pos.y, this.pos.x - this.vel.x, this.pos.y - this.vel.y);
  }
}

class Explosion {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.radius = 0;
    this.maxRadius = 250;
    this.lifespan = 255;
  }

  update() {
    this.radius += 15;
    this.lifespan -= 10;
  }

  isDone() {
    return this.lifespan <= 0;
  }

  affect(particle) {
    let d = dist(particle.pos.x, particle.pos.y, this.pos.x, this.pos.y);
    if (d < this.radius) {
      let dir = p5.Vector.sub(particle.pos, this.pos);
      dir.normalize();
      let strength = map(this.radius - d, 0, this.radius, 4, 0);
      dir.mult(strength);
      particle.applyForce(dir);
    }
  }

  kills(particle) {
    let d = dist(particle.pos.x, particle.pos.y, this.pos.x, this.pos.y);
    return d < this.radius * 0.2;
  }

  show() {
    noFill();
    stroke(255, 100, 100, this.lifespan);
    strokeWeight(3);
    ellipse(this.pos.x, this.pos.y, this.radius * 2);
  }
}
```
### Pieza final 

![image](https://github.com/user-attachments/assets/bb1d1d3d-8a8e-4b8d-83f5-ad2dbef7c45f)



