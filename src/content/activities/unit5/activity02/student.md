**SIMULACIONES A ANALIZAR:**

## 1. Revisa detalladamente el ejemplo 4.2: an [Array of Particles](https://natureofcode.com/particles/#example-42-an-array-of-particles).

**Código original**

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria?

* **Asignación de memoria**

1. Cada vez que creamos una Particle, se reserva espacio en la memoria para su posición, velocidad, aceleración y otros atributos.
2. Si no elimináramos partículas, el array particles crecería infinitamente, causando problemas de rendimiento.

* **Liberación de memoria**
1. Cuando una partícula muere, se elimina del array con splice (i, 1), lo que hace que JavaScript libere su referencia en memoria y permita que el garbage collector la elimine en algún momento. Este enfoque es eficiente porque JavaScript maneja la memoria automáticamente, pero si el número de partículas fuera muy alto, podríamos optimizarlo más.

**Código modificado**

![image](https://github.com/user-attachments/assets/bf789bde-02f1-4354-90bd-2d7b5c9e6f18)

![image](https://github.com/user-attachments/assets/4a0d0951-2e23-4567-9972-ef2786ccd7a6)

[Simulación aquí](https://editor.p5js.org/WatermelonSuggar/sketches/xASgHG0km)

**particle.js**

```js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.applyAttraction(mouseX, mouseY);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  applyAttraction(targetX, targetY) {
    let target = createVector(targetX, targetY);
    let force = p5.Vector.sub(target, this.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 80);
    force.normalize();
    let strength = 8 / distance;
    force.mult(strength);
    this.applyForce(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan <= 0;
  }
}


```
**sketch.js**

```js
let particles = [];
const MAX_PARTICLES = 200;
const MIN_PARTICLES = 50; // Para evitar que se queden sin partículas

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);

  // Calculamos la velocidad del mouse
  let mouseSpeed = dist(mouseX, mouseY, pmouseX, pmouseY);
  
  // Mapeamos la velocidad del mouse a la cantidad de partículas creadas
  let particlesToAdd = map(mouseSpeed, 0, 50, 1, 5); // De 1 a 5 partículas

  // Agregar partículas según la velocidad del mouse
  if (particles.length < MAX_PARTICLES) {
    for (let i = 0; i < particlesToAdd; i++) {
      particles.push(new Particle(mouseX, mouseY));
    }
  }

  // Recorrer partículas y actualizar
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();

    // Solo eliminar si hay suficientes partículas para mantener la fluidez
    if (particle.isDead() && particles.length > MIN_PARTICLES) {
      particles.splice(i, 1);
    }
  }
}

```

> 🌳Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.

* Hice que las partículas aparezcan y desaparezcan de manera controlada. Con este sistema, evitamos que el programa se vuelva lento por tener demasiadas partículas al mismo tiempo.

* Creación:
  * Cada vez que el programa dibuja un nuevo cuadro, revisa qué tan rápido se está moviendo el mouse.
  * Si el mouse está quieto, aparecen pocas partículas.
  * Si el mouse se mueve rápido, aparecen más.
  * Las partículas se crean justo en la posición del mouse y empiezan a moverse solas.

* Desaparición:
  * Cada partícula tiene un "tiempo de vida" que se va reduciendo con el tiempo.
  * Cuando su tiempo de vida llega a 0, la eliminamos del arreglo que las guarda.
  * Si ya hay muchas partículas, también eliminamos algunas para que la pantalla no se llene demasiado.

> 🌳Explica qué concepto aplicaste, cómo lo aplicaste y por qué.

**Concepto aplicado**
*  Apliqué una **fuerza de atracción inversamente proporcional a la distancia**, es decir que, si la partícula está lejos del mouse, la atracción es más débil. Si la partícula está cerca del mouse, la atracción es más fuerte.

**Concepto aplicado y cómo lo apliqué**

* Calculé la dirección hacia el mouse, para cada partícula, obtuve un vector que apunta desde su posición hasta el mouse.
* Medí la distancia entre la partícula y el mouse, esto me permite ajustar la intensidad de la atracción.
* Normalicé el vector para que solo indique dirección, así evité que partículas muy lejos reciban una fuerza excesiva.
* Multipliqué la fuerza por un valor inversamente proporcional a la distancia
* Apliqué la fuerza a la partícula, lo que hace que la partícula cambie su aceleración y se mueva hacia el mouse.


**¿Por qué?**

*  Para que las partículas reaccionen al mouse de forma natural en lugar de moverse de manera brusca.
*  Si usara una fuerza constante, todas las partículas se moverían igual sin importar la distancia y haría que la simulación fuera monótona.
*  Para mejorar la visualización de la simulación, dándole una mejor interacción y dinamismo con el mouse.

_________________________________________________________________________________


## 2. Analiza el ejemplo 4.4: a [System of Systems.](https://natureofcode.com/particles/#example-44-a-system-of-systems)

**Código original**

> ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?

* Creación:
  * Cada vez que se crea un Emisor (Emitter), se inicializa en una posición específica.
  * En cada ciclo de draw(), los emisores generan nuevas partículas en su posición usando addParticle().
  * Estas partículas se agregan a la lista this.particles dentro de cada Emitter.

* Desaparición:
  * Cada partícula tiene una variable _lifespan_ que disminuye en cada frame.
  * Cuando lifespan llega a 0, la partícula se considera "muerta".
  * En run(), se revisa cada partícula con isDead(), y si está muerta, se elimina con splice(i,1). Esto se hace recorriendo el array de atrás hacia adelante (for (let i = this.particles.length - 1; i >= 0; i--)), evitando errores al eliminar elementos dentro de un bucle.

* Gestión de memoria en la simulación
  * Se usa splice(i, 1) para eliminar partículas que han agotado su tiempo de vida. Esto evita acumulación innecesaria de objetos en la memoria, manteniendo el código eficiente.
  * Cada Emitter maneja su propia lista de partículas de forma independiente.
  * No hay eliminación automática de emisores, lo que podría hacer que la simulación consuma más memoria con el tiempo si se crean demasiados.
  * No se almacenan partículas innecesarias.

**Código modificado**

[Simulación aquí](https://editor.p5js.org/WatermelonSuggar/sketches/SgkKsbJCe)

![image](https://github.com/user-attachments/assets/b14fd425-7502-4c00-b33e-dd7da52ac494)

**emitter.js**

```js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run(gravity, time) {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run(gravity, time);
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

```

**particle.js**

```js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;

    // Tamaño usando vuelo de Lévy
    this.size = this.levySize();

    // Offsets para Perlin Noise en color
    this.colorOffsetR = random(1000);
    this.colorOffsetG = random(1000);
    this.colorOffsetB = random(1000);
  }

  run(gravity, time) {
    this.applyForce(gravity);
    this.levyFlight();
    this.update();
    this.show(time);
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  levyFlight() {
    if (random() < 0.1) {
      let angle = random(TWO_PI);
      let step = pow(random(1), -1.5); // Vuelo de Lévy
      this.velocity.add(p5.Vector.fromAngle(angle).mult(step));
    }
  }

  levySize() {
    // Generamos un tamaño usando vuelo de Lévy
    let step = pow(random(1), -1.5) * 5; // Factor para escalar el tamaño
    return constrain(step, 5, 20); // Limitamos a un rango razonable
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show(time) {
    let r = map(noise(this.colorOffsetR + time * 0.01), 0, 1, 50, 255);
    let g = map(noise(this.colorOffsetG + time * 0.01), 0, 1, 50, 255);
    let b = map(noise(this.colorOffsetB + time * 0.01), 0, 1, 50, 255);

    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(r, g, b, this.lifespan);
    circle(this.position.x, this.position.y, this.size);
  }

  isDead() {
    return this.lifespan < 0.0 || this.position.y > height || this.position.x < 0 || this.position.x > width;
  }
}
```

**sketch.js**

```js
let emitters = [];
let gravity;
let time = 0; // Para variar el ruido de Perlin con el tiempo

function setup() {
  createCanvas(640, 240);
  gravity = createVector(0, 0.05);
}

function draw() {
  background(255);
  time += 1;

  for (let emitter of emitters) {
    emitter.run(gravity, time);
    if (mouseIsPressed) { // Solo genera si el mouse está presionado
      emitter.addParticle();
    }
  }
}

function mousePressed() {
  emitters = []; // Elimina todos los emitters anteriores
  let emitter = new Emitter(mouseX, mouseY);
  emitters.push(emitter);
  for (let i = 0; i < 10; i++) {
    emitter.addParticle();
  }
}
```

> 🌳Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.

* Creación de partículas:
  * Cada vez que el usuario hace clic, se genera un nuevo Emitter en la posición del mouse, y eliminamos cualquier Emitter anterior para evitar acumulaciones innecesarias. Esto significa que siempre hay un único Emitter en pantalla.
  * Dentro del Emitter, las partículas se crean de dos formas: Cuando se hace clic, se generan 10 partículas iniciales. Mientras el mouse está presionado, el Emitter sigue generando nuevas partículas.

* Desaparición:
  * Cada partícula tiene una vida útil (lifespan) que disminuye con el tiempo. También se eliminan si salen del área del canvas.
  * En cada frame, revisamos si una partícula ha "muerto" y la eliminamos de la lista, lo que ayuda a mantener el rendimiento y la eficiencia en el uso de la memoria.

> 🌳Explica qué concepto aplicaste, cómo lo aplicaste y por qué.

**Concepto aplicado y cómo lo apliqué**

*  Apliqué distintas distribuciones:
    *  **Ruido de Perlin para el color** ya que cada partícula tiene valores únicos para R, G y B, que se generan usando Perlin Noise. Estos valores cambian con el tiempo para crear transiciones de color más suaves y orgánicas.
    *  **Distribución normal para la velocidad inicial,**  lugar de asignar velocidades completamente aleatorias, utilicé una distribución normal para que la mayoría de las partículas tengan velocidades cercanas a un valor promedio, con algunas pocas siendo mucho más rápidas o lentas.
    *  **Vuelo de Lévy para el tamaño y movimiento** porque el tamaño de las partículas se genera con una distribución de Vuelo de Lévy, lo que significa que la mayoría son pequeñas, pero ocasionalmente aparecen algunas más grandes. También se aplica a su movimiento para que, en ocasiones, las partículas hagan saltos grandes en direcciones aleatorias.
 

**¿Por qué?**

* El ruido de perlin: genera cambios suaves y naturales, evitando cambios bruscos de color.
* Distribución normal: Hace que la dispersión de las partículas sea más realista pues en el mundo real, la mayoría de los fenómenos físicos siguen distribuciones normales.
* Vuelo de Lévy: Hace que algunas partículas realicen "saltos" grandes en lugar de moverse de manera uniforme generando un efecto más dinámico e impredecible y aumentando la variabilidad visual.

______________________________________________________________________________________________________________________________________

## 3. Analiza el ejemplo 4.5: [a Particle System with Inheritance and Polymorphism.](https://natureofcode.com/particles/#example-45-a-particle-system-with-inheritance-and-polymorphism)

**Código original**

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria?

* Creación:
  * En cada frame (draw() en sketch.js), se llama a emitter.addParticle(), lo que añade una nueva partícula al sistema.
  * La partícula puede ser un Confetti (cuadrado) o una Particle (círculo), con un 50% de probabilidad para cada una.
  * Todas las partículas inician en la misma posición this.origin (el punto del emisor).

*  Desaparición:
  * En cada frame run() se actualiza cada partícula (p.run()), verificando si su ciclo de vida lifespan es menor a 0.
  * Si la partícula "ha muerto", se elimina del array de particles con splice(i,1). 
  
*  Gestión de memoria: Se eliminan del array las partículas inactivas para evitar la acumulación de memoria.


**Código modificado**

[Simulación modificada aquí](https://editor.p5js.org/WatermelonSuggar/sketches/i3wXsccLr)

![image](https://github.com/user-attachments/assets/f787440f-e03b-4327-8681-52f2743e6ae9)

**confetti.js**

```js
class Confetti extends Particle {
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);
    rectMode(CENTER);
    fill(this.painted ? color(128, 0, 128) : color(127, this.lifespan));
    stroke(0, this.lifespan);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}
```

**emitter.js**

```js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
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
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
    this.size = 8;
    this.painted = false;
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
    fill(this.painted ? color(128, 0, 128) : color(127, this.lifespan));
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

    this.angleVelocity = 0.0;
    this.angleAcceleration = 0.0;
    this.damping = 0.995; // Amortiguación
    this.ballr = 20; // Radio del bob
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

    stroke(0);
    strokeWeight(2);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);
    fill(128, 0, 128);
    noStroke();
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
      this.angle = atan2(-diff.y, diff.x) - PI / 2;
    }
  }

  checkCollision(particles) {
    for (let p of particles) {
      let d = dist(this.bob.x, this.bob.y, p.position.x, p.position.y);
      if (d < this.ballr + p.size / 2) {
        p.painted = true;
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
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 20);
  pendulum = new Pendulum(width / 2, 20, 150);
}

function draw() {
  background(255);

  emitter.addParticle();
  emitter.run();

  pendulum.update();
  pendulum.drag();
  pendulum.checkCollision(emitter.getParticles()); // Detectar colisión con partículas
  pendulum.show();
}

function mousePressed() {
  pendulum.clicked(mouseX, mouseY);
}

function mouseReleased() {
  pendulum.stopDragging();
}


```

> 🌳Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.

* No realicé ninguna modificación en cuanto a la generación de partículas porque quería realizar los cambios desde la integración de un nuevo concepto visto en unidades anteriores. Así que las partículas se siguen generando gracias al emitter, siguen estando las posibilidades 50/50 de que la partícula sea un cuadrado (cofetti.js) o un círculo en (particle.js). Y se siguen desapareciendo si su ciclo de vida es menor a 0, para evitar saturar la memoria.

> 🌳Explica qué concepto aplicaste, cómo lo aplicaste y por qué.

**Concepto aplicado y cómo lo apliqué**

* Regresé al concepto del **péndulo**. La idea era que en la parte inferior del canvas estuviera un péndulo que al entrar en contacto con las partículas que se generaban gracias al emitter, las pintara de color uva. 

Creé una clase pendulum.js para que contuviera al péndulo simple y en esta misma clase hay una función checkCollision() que analiza si la partícula entra en contacto con el bob y si es así lo pinta del color indicado.

**¿Por qué?**

* Quería aplicar el concepto de resorte pero me di cuenta que no era óptimo para este ejemplo, incluso si era un resorte simple. Así que decidí replantear el concepto y fue allí donde me percaté que el movimiento natural de un péndulo podría sin mucho esfuerzo crear una buena interacción con las partículas.
______________________________________________________________________________________________________________________________________

## 4. Analiza el ejemplo 4.6: [a Particle System with Forces](https://natureofcode.com/particles/#example-46-a-particle-system-with-forces).

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria?

**Código original**

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

* Creación: Sigue funcionando con el ciclo **draw()**, el **emitter()** y el **addParticle()** y el número de partículas crece indefinidamente hasta que son eliminadas.

  * Se instancia un nuevo objeto Particle, se almacena en el array particles[] dentro del Emitter.
  * Cada partícula tiene una posición inicial (this.origin.x, this.origin.y).
  * Se crea con una velocidad inicial aleatoria (random(-1, 1) en X y random(-2, 0) en Y).

* Gestión de la memoria y eliminación de partículas:
  * Cada partícula tiene una propiedad lifespan (inicialmente 255) que disminuye con el tiempo en update().
  * Cuando lifespan < 0, la partícula es considerada "muerta". Para eliminarla, en Emitter.run() se usa un bucle inverso (for de atrás hacia adelante) para recorrer el array particles[].
  * Si particle.isDead() retorna true (cuando lifespan < 0), la partícula se elimina con splice(i, 1).
  * Se recorre el array de forma inversa para evitar problemas con los índices al eliminar elementos.

**Código modificado**

![image](https://github.com/user-attachments/assets/ea0011f0-cd66-46c2-a380-e18183c48302)

[Simulación aquí](https://editor.p5js.org/WatermelonSuggar/sketches/NUSzpJyE7)

**emitter.js**

```js
class Emitter {
  constructor(x, y, inverted = false) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.inverted = inverted; // Indica si es el emisor reflejado
  }

  addParticle() {
    let xOffset = randomGaussian(0, 40);
    let yOffset = randomGaussian(0, 25);

    let x = this.origin.x + xOffset;
    let y = this.origin.y + (this.inverted ? -yOffset : yOffset); // Invertimos para el reflejado

    this.particles.push(new Particle(x, y, this.inverted));
  }

  applyForce(force) {
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```

**particle.js**

```js
class Particle {
  constructor(x, y, inverted = false) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0.0);
    this.velocity = createVector(random(-1, 1), random(-2, 0));
    if (inverted) this.velocity.y *= -1; // Invierte el movimiento si es el reflejado
    this.lifespan = 255.0;
    this.mass = 1;

    this.r = constrain(randomGaussian(map(x, 0, width, 50, 255), 80), 0, 255);
    this.g = constrain(randomGaussian(map(y, 0, height, 50, 255), 80), 0, 255);
    this.b = constrain(randomGaussian(150, 80), 0, 255);
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2.0;
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(this.r, this.g, this.b, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}
```
**sketch.js**

```js
let emitter, emitter2;

function setup() {
  createCanvas(640, 640);
  emitter = new Emitter(width / 2, 50); // Emisor original
  emitter2 = new Emitter(width / 2, height - 50, true); // Emisor reflejado
}

function draw() {
  background(255, 30);

  let gravity = createVector(0, 0.1);
  let antiGravity = createVector(0, -0.1); // Para el reflejado

  emitter.applyForce(gravity);
  emitter2.applyForce(antiGravity); // Aplica fuerza hacia arriba

  emitter.addParticle();
  emitter2.addParticle();

  emitter.run();
  emitter2.run();
}
```
> 🌳Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.

* Creación de partículas:
  * Tengo dos sistemas de partícula y en cada cuadro draw() se crean nuevas partículas.
  * emitter.addParticle() agrega una nueva partícula al sistema original y emitter2.addParticle() agrega las partículas al sistema reflejado.

* Eliminación:
  * Cada partícula tiene una vida útil, la variable lifespan empieza en 255.0 y se reduce en cada frame.
  * Se recorre el array de partículas al revés para evitar problemas al eliminar elementos dentro del bucle.

> 🌳Explica qué concepto aplicaste, cómo lo aplicaste y por qué.

**Concepto aplicado y cómo lo apliqué**

* En esta simulación, utilicé la distribución normal (o gaussiana) para generar la posición inicial de las partículas y su color. Este tipo de distribución es útil cuando queremos que los valores generados se concentren alrededor de una media, con una probabilidad decreciente a medida que nos alejamos de ella.

**¿Por qué?**

* Quería simular una cascada y variar un poco la posición en la que se generaban de manera que no fueran muy abruptos los cambios y me pareció divertido que pudieran reflejarse y caer una sobre la otyra pero con distintos colores según la posición. Para la posición, usé randomGaussian() para que las partículas se concentren cerca del emisor con una dispersión natural. Para el color, usé la misma función para generar variaciones suaves en los valores RGB en función de la posición.

Esto permite una simulación más realista, donde la mayoría de las partículas aparecen cerca del origen y solo algunas se alejan, evitando distribuciones artificiales.


______________________________________________________________________________________________________________________________________

## 5. Analiza el ejemplo 4.7: [a Particle System with a Repeller](https://natureofcode.com/particles/#example-47-a-particle-system-with-a-repeller).

**Código original**

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria?

* Creación:
  *  En cada fotograma (draw()), se llama emitter.addParticle(), que crea una nueva partícula en la posición del emisor.
  *  Esto significa que una nueva partícula se añade al arreglo particles en cada frame, manteniendo el sistema en constante generación.
  *  A todas las partículas vivas se les aplica una fuerza de gravedad constante hacia abajo. Una fuerza de repulsión desde el Repeller, que varía dependiendo de la distancia a cada partícula.

* Desaparición y control de memoria:
  * En el método run() de Emitter, se recorre el arreglo particles de atrás hacia adelante.
  * Cada partícula tiene un atributo lifespan, que disminuye con el tiempo en update(). Cuando este valor cae por debajo de 0, se considera "muerta".

**Código modificado**

[Simulación aquí](https://editor.p5js.org/WatermelonSuggar/sketches/CXaQZzGTn)

![image](https://github.com/user-attachments/assets/cc1211ab-1182-4a0b-b03c-7d1d683bd524)

**emitter.js**

```js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.timer = 0;
  }

  addParticle() {
    this.timer++;
    if (this.timer % 100 === 0) {
      this.particles.push(new VectorParticle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    }
  }

  applyForce(force) {
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  applyRepeller(repeller) {
    for (let particle of this.particles) {
      let force = repeller.repel(particle);
      particle.applyForce(force);
    }
  }

  applyVectorRepel() {
    let vectors = this.particles.filter(p => p instanceof VectorParticle);
    for (let vector of vectors) {
      for (let particle of this.particles) {
        if (particle !== vector) {
          let repelForce = vector.repel(particle);
          particle.applyForce(repelForce);
        }
      }
    }
  }

  run() {
    this.applyVectorRepel();
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

```
**particle.js**

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255.0;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(f) {
    this.acceleration.add(f);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}

```
**sketch.js**

```js
let emitter;


function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 60);
  
}

function draw() {
  background(255);
  emitter.addParticle();

  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);

  emitter.run();

}
```
> 🌳Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.

* No cambié nada de la generación, eliminación y gestión de partículas en la memoria.

> 🌳Explica qué concepto aplicaste, cómo lo aplicaste y por qué.

* Apliqué una distribución gaussiana para generar variaciones suaves y naturales en dos aspectos del sistema de partículas: la posición inicial de las partículas y el color 

**Concepto aplicado y cómo lo apliqué**

* Vectores que representan fuerzas como la gravedad y la repulsión. También apliqué el motion 101 porque cada partícula tiene una posición, una velocidad y una aceleración
  
* Partículas especiales con repulsión, inspiradas en la ley de Coulomb que repelen a las demás usando una fuerza basada en la distancia.

**Por qué**

* Quería explorar con los vectores como entes de repulsión para ver cómo interactuaba una partícula especial con las normales.






_____________________________________________________________________________________________________________________________________
