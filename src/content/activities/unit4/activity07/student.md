**Simulación con un oscilador de objetos**

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/QYPiLLW3j)


**Activación del modo atracción**

**Estado:** Verde (Modo de Atracción)

**Comportamiento:** El botón está en verde, lo que indica que el modo de atracción está activado.

**Osciladores:** Los osciladores son atraídos hacia el centro de la pantalla (en la posición (width / 2, height / 2)).

**Movimiento:** Los osciladores se mueven de forma más suave y lenta, siguiendo una fuerza de atracción suave hacia el centro del lienzo, calculada a través de p5.Vector.sub(center, this.position).setMag(0.05). La velocidad de oscilación es más baja en este modo lo que simula un congelamiento.

**Fuerza de atracción:** La atracción hacia el centro está activada, pero los osciladores se mantienen estáticos con pequeños movimientos en lugar de moverse libremente. Esto da la apariencia de que las bolas están "congeladas" con pequeños movimientos aleatorios.

**Fondo:** El fondo se pone negro.

**Color de las esferas:** El color de las esferas cambia a azul claro, como un efecto visual que indica que están en el modo de atracción.

![image](https://github.com/user-attachments/assets/aa6924ae-8a58-4418-84b6-9eca75663ea0)


**Modo desactivado - locura total**

![image](https://github.com/user-attachments/assets/0098ddaf-4749-4247-b2f5-9035f36980ac)

**Estado del botón:** Rojo (Modo Normal)

**Comportamiento:** El botón está en rojo, lo que indica que el modo de atracción está desactivado.

**Osciladores:** Los osciladores están libres de la atracción al centro y se mueven aleatoriamente con una velocidad de oscilación más alta, determinada por las fuerzas aleatorias aplicadas a angleVelocity en la función update().

**Movimiento:** Las esferas se mueven de forma más caótica, impulsadas por la aleatoriedad en sus velocidades (angleVelocity). Este comportamiento genera trayectorias erráticas y dinámicas.

**Fuerza de atracción:** No hay fuerza de atracción hacia el centro del lienzo.

**Fondo:** El fondo es blanco.

**Color de las esferas:** El color de las esferas está determinado por el ruido gaussiano, lo que da lugar a una mezcla de colores morados (usando el valor del ruido para calcular el color de las bolas).


**Código - sketch.js**

```js
// An array of objects
let oscillators = [];
let attractMode = false; // Controla si la fuerza de atracción está activada

function setup() {
  createCanvas(640, 240);
  // Inicializar todos los osciladores
  for (let i = 0; i < 10; i++) {
    oscillators.push(new Oscillator());
  }
}

function draw() {
  // Cambiar fondo y color de las bolitas dependiendo del estado
  if (attractMode) {
    background(0); // Fondo negro cuando las partículas están atraídas
  } else {
    background(255); // Fondo blanco cuando no están atraídas
  }

  // Dibujar el botón circular
  fill(attractMode ? 'green' : 'red');
  noStroke();
  ellipse(40, 40, 30, 30); // Posición y tamaño del botón

  // Verificar colisiones entre osciladores
  for (let i = 0; i < oscillators.length; i++) {
    for (let j = i + 1; j < oscillators.length; j++) {
      oscillators[i].checkCollision(oscillators[j]);
    }
  }

  // Actualizar y mostrar los osciladores
  for (let osc of oscillators) {
    osc.update();
    osc.show();
  }
}

// Detectar clic en el botón
function mousePressed() {
  let d = dist(mouseX, mouseY, 40, 40);
  if (d < 15) { // Verifica si se hizo clic en el círculo
    attractMode = !attractMode; // Alterna el estado del modo de atracción
    for (let osc of oscillators) {
      osc.isAttracted = attractMode; // Actualiza la fuerza de atracción de los osciladores
    }
  }
}

```
**Código - oscillator.js**

```js
class Oscillator {
  constructor() {
    this.angle = createVector();
    this.angleVelocity = createVector(random(-0.05, 0.05), random(-0.05, 0.05));
    this.angleAcceleration = createVector(0, 0);
    this.amplitude = createVector(
      random(20, width / 2),
      random(20, height / 2)
    );
    this.position = createVector(width / 2, height / 2);
    this.radius = 16; // Radio de la esfera
    this.isAttracted = false; // Variable para saber si está siendo atraído
    this.baseColor = color(random(100, 255), random(50, 150), random(200, 255)); // Color base de la bolita
  }

  update() {
    // Aplicar la fuerza de atracción si está activada
    if (this.isAttracted) {
      let center = createVector(width / 2, height / 2);
      let force = p5.Vector.sub(center, this.position);
      force.setMag(0.05); // Fuerza de atracción suave
      this.angleVelocity.add(force);
    } else {
      // Simular una fuerza aleatoria
      this.angleAcceleration = createVector(random(-0.001, 0.001), random(-0.001, 0.001));
      this.angleVelocity.add(this.angleAcceleration);
      this.angle.add(this.angleVelocity);
    }

    // Caminata aleatoria en la amplitud
    this.amplitude.x += random(-1, 1);
    this.amplitude.y += random(-1, 1);
    this.amplitude.x = constrain(this.amplitude.x, 20, width / 2);
    this.amplitude.y = constrain(this.amplitude.y, 20, height / 2);

    // Actualizar la posición
    this.position.x = width / 2 + sin(this.angle.x) * this.amplitude.x;
    this.position.y = height / 2 + sin(this.angle.y) * this.amplitude.y;

    // Rebote en los bordes
    if (this.position.x - this.radius < 0 || this.position.x + this.radius > width) {
      this.angleVelocity.x *= -1;
    }
    if (this.position.y - this.radius < 0 || this.position.y + this.radius > height) {
      this.angleVelocity.y *= -1;
    }
  }

  show() {
    let speed = this.angleVelocity.mag();
    let col;

    if (this.isAttracted) {
      // Si está siendo atraído, el color es azul
      col = color(60, 204, 254);
    } else {
      // Si no está siendo atraído, el color varía con el ruido gaussiano
      let noiseValue = randomGaussian();
      col = color(map(noiseValue, -3, 3, 100, 255), map(noiseValue, -3, 3, 0, 100), map(noiseValue, -3, 3, 150, 255));
    }

    fill(col);
    push();
    stroke(0);
    strokeWeight(2);
    line(width / 2, height / 2, this.position.x, this.position.y);
    circle(this.position.x, this.position.y, this.radius * 2);
    pop();
  }

  checkCollision(other) {
    let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
    if (d < this.radius * 2) {
      // Vectores de diferencia de posición y velocidad
      let normal = p5.Vector.sub(other.position, this.position).normalize();
      let relativeVelocity = p5.Vector.sub(this.angleVelocity, other.angleVelocity);

      // Producto punto entre la velocidad relativa y la normal
      let velocityAlongNormal = relativeVelocity.dot(normal);

      // Si las esferas se están acercando
      if (velocityAlongNormal > 0) {
        return; // No hay colisión
      }

      // Coeficiente de restitución (elasticidad perfecta, e = 1)
      let e = 1; 

      // Cálculo de los cambios de velocidad
      let impulse = (2 * velocityAlongNormal) / (this.radius + other.radius);
      let impulseVector = normal.mult(impulse);

      // Actualizar las velocidades de las esferas
      this.angleVelocity.sub(impulseVector);
      other.angleVelocity.add(impulseVector);
    }
  }
}
```

**Código - oscillator.js**
