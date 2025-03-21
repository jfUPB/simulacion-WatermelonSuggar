Obra de arte generativa algorítmica interactiva en tiempo real
Enunciado: diseña e implementa una obra de arte generativa algorítmica interactiva en tiempo real en p5.js que cumpla con los siguientes requisitos:

Selecciona uno de los conceptos con los que experimentaste en la fase de investigación y propón la obra alrededor de este.
La obra debe ser interactiva en tiempo real. Puedes usar teclado, mouse, música, el micrófono, video, sensor o cualquier otro dispositivo de entrada.
Documenta el proceso de creación, incluyendo la idea inicial, bocetos, experimentación con el código y el resultado final.
 
> **PROCESO CREATIVO**

* **Idea inicial**
  
  * Quisiera explorar con el concepto de ondas, con la suavidad de su movimiento y con la fuerza de atracción hacia un atractor. Me inspiré en imáganes de referencia en Pinterest y en el video [Generative Art](https://www.youtube.com/watch?v=qtPi0JvmWbs) de Hao Hua.

**Imágenes de referencia**

![image](https://github.com/user-attachments/assets/5c0668e9-b3c4-4110-9037-6809de33805b)

![image](https://github.com/user-attachments/assets/40452e6b-d5e6-42de-ac63-fc8b0a5cddaf)

![image](https://github.com/user-attachments/assets/22f753fc-ac0a-4177-b3ff-c0e35c0844b0)

* **Experimentación con el código**
  
Realicé varias versiones del proyecto hasta crear uno que me hiciera sentir cómoda con el resultado pues quería que se sintiera fluido y pausado para que diera espacio a la contemplación de la obra.
 
  * [Versión 1](https://editor.p5js.org/WatermelonSuggar/sketches/7xBKGM9Bg)
 
  ![image](https://github.com/user-attachments/assets/535f76b6-b3ad-431d-b4fb-b34e0b8ec613)

  * [Versión 2](https://editor.p5js.org/WatermelonSuggar/sketches/9GmYl6kcj)

  ![image](https://github.com/user-attachments/assets/d0af7394-8294-4c23-a6d6-b47fbfb06a71)

  * [Versión 3](https://editor.p5js.org/WatermelonSuggar/sketches/O8e_XuJgW)

   ![image](https://github.com/user-attachments/assets/2415ed9a-e7bd-4192-93cd-5082e2a37df9)



* **Resultado final**

 * Finalmente decidí explorar las funciones sinuoidales y la interacción sucede al pasar el mouse sobre la simulación, el color de las partículas varía. En un principio son azules y si paso el mouse sobre ellas se vuelven naranjas gracias a una rampa de color.

[Versión FINAL](https://editor.p5js.org/WatermelonSuggar/sketches/iPXaAoNYK)
 
![image](https://github.com/user-attachments/assets/a95f6fe4-e58a-4c3a-a83a-3b12b5ad4753)

![image](https://github.com/user-attachments/assets/eac19b75-e443-45b4-a7a5-28f4236ce0a5)



**Código**

```js
let particles = [];
let flowField;
let cols, rows;
let scl = 20;
let inc = 0.1;

function setup() {
  createCanvas(windowWidth, windowHeight);
  cols = floor(width / scl);
  rows = floor(height / scl);
  flowField = new Array(cols * rows);
  
  for (let i = 0; i < 5000; i++) {
    particles.push(new Particle());
  }
}

function draw() {
  background(0, 10);
  let yoff = 0;
  for (let y = 0; y < rows; y++) {
    let xoff = 0;
    for (let x = 0; x < cols; x++) {
      let index = x + y * cols;
      let angle = noise(xoff, yoff) * TWO_PI * 4;
      let v = p5.Vector.fromAngle(angle);
      flowField[index] = v;
      v.setMag(1);
      xoff += inc;
    }
    yoff += inc;
  }

  for (let particle of particles) {
    particle.follow(flowField);
    particle.update();
    particle.edges();
    particle.show();
  }
}

class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxSpeed = 2;
    this.prevPos = this.pos.copy();
    
    // Definir si la partícula sigue el mouse de forma sinusoidal o directa
    this.attractionFactor = random(0.2, 1);
    this.sinusoidal = this.attractionFactor > 0.6; // 60% en zigzag

    // Parámetros personalizados para el movimiento sinusoidal
    this.sinAmp = random(3, 15);  // Amplitud entre 3 y 15 px
    this.freq = random(0.05, 0.3); // Frecuencia entre 0.05 y 0.3
    this.phaseOffset = random(TWO_PI); // Fase inicial diferente para cada partícula
  }

  follow(vectors) {
    let x = floor(this.pos.x / scl);
    let y = floor(this.pos.y / scl);
    let index = x + y * cols;
    let force = vectors[index];
    this.applyForce(force);
  }

  applyForce(force) {
    this.acc.add(force);
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

    let attractionStrength = map(distToMouse, 0, width, 0.05, 0);
    let angle = atan2(mouseY - this.pos.y, mouseX - this.pos.x);

    let forceX, forceY;

    if (this.sinusoidal) {
      // Movimiento en zigzag con parámetros personalizados
      let sinOffsetX = sin(frameCount * this.freq + this.phaseOffset) * this.sinAmp;
      let sinOffsetY = cos(frameCount * this.freq + this.phaseOffset) * this.sinAmp;

      forceX = cos(angle) * attractionStrength * 5 + sinOffsetX;
      forceY = sin(angle) * attractionStrength * 5 + sinOffsetY;
    } else {
      // Movimiento directo
      forceX = cos(angle) * attractionStrength * 5;
      forceY = sin(angle) * attractionStrength * 5;
    }

    this.vel.x += forceX;
    this.vel.y += forceY;

    // Color cambia con la distancia al mouse
    let colorChange = map(distToMouse, 0, width, 255, 0);
    stroke(colorChange, 100, 255 - colorChange, 100);

    // Dibujar líneas que muestran el recorrido
    strokeWeight(1);
    line(this.pos.x, this.pos.y, this.pos.x - this.vel.x, this.pos.y - this.vel.y);
  }
}

```
