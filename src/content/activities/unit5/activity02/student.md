**SIMULACIONES A ANALIZAR:**

## 1. Revisa detalladamente el ejemplo 4.2: an [Array of Particles](https://natureofcode.com/particles/#example-42-an-array-of-particles).

**Código original**

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

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

**Cómo lo apliqué**

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

> ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

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

**Concepto aplicado**
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

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

* Creación:
  * En cada frame (draw() en sketch.js), se llama a emitter.addParticle(), lo que añade una nueva partícula al sistema.
  * La partícula puede ser un Confetti (cuadrado) o una Particle (círculo), con un 50% de probabilidad para cada una.
  * Todas las partículas inician en la misma posición this.origin (el punto del emisor).

*  Desaparición:
  * En cada frame run() se actualiza cada partícula (p.run()), verificando si su ciclo de vida lifespan es menor a 0.
  * Si la partícula "ha muerto", se elimina del array de particles con splice(i,1). 
  
*  Gestión de memoria: Se eliminan del array las partículas inactivas para evitar la acumulación de memoria.


**Código modificado**

[Simulación modificada aquí](https://editor.p5js.org/WatermelonSuggar/sketches/MD0prJkgp)

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

> 🌳Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.

* No realicé ninguna modificación en cuanto a la generación de partículas porque quería realizar los cambios desde la integración de un nuevo concepto visto en unidades anteriores. Así que las partículas se siguen generando gracias al emitter, siguen estando las posibilidades 50/50 de que la partícula sea un cuadrado (cofetti.js) o un círculo en (particle.js). Y se siguen desapareciendo si su ciclo de vida es menor a 0, para evitar saturar la memoria.

> 🌳Explica qué concepto aplicaste, cómo lo aplicaste y por qué.

**Concepto aplicado**

* Regresé al concepto del **péndulo**. La idea era que en la parte inferior del canvas estuviera un péndulo que al entrar en contacto con las partículas que se generaban gracias al emitter, las pintara de color uva. 

Creé una clase pendulum.js para que contuviera al péndulo simple y en esta misma clase hay una función checkCollision() que analiza si la partícula entra en contacto con el bob y si es así lo pinta del color indicado.

**¿Por qué?**

* Quería aplicar el concepto de resorte pero me di cuenta que no era óptimo para este ejemplo, incluso si era un resorte simple. Así que decidí replantear el concepto y fue allí donde me percaté que el movimiento natural de un péndulo podría sin mucho esfuerzo crear una buena interacción con las partículas.
______________________________________________________________________________________________________________________________________

✅ 4. Analiza el ejemplo 4.6: [a Particle System with Forces](https://natureofcode.com/particles/#example-46-a-particle-system-with-forcescha).

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

**Código original**

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

______________________________________________________________________________________________________________________________________

✅ 5. Analiza el ejemplo 4.7: a Particle System with a Repeller.

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

_____________________________________________________________________________________________________________________________________
