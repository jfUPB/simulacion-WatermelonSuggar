**Modelamiento de fuerzas**

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/WvUgroOAL)

```js

let mover;
let select;
let liquid;
let attractor;
let attracting = false;

function setup() {
  createCanvas(600, 400);
  mover = new Mover(50, 200, 2);
  attractor = new Attractor(width / 2, height / 2, 20);
  
  select = createSelect();
  select.position(10, 10);
  select.option('Fricción');
  select.option('Resistencia del aire');
  select.option('Resistencia de fluidos');
  select.option('Atracción gravitacional');
  select.changed(resetMover);
  
  liquid = new Liquid(0, height / 2, width, height / 2, 0.1);
}

function draw() {
  background(220);
  let selectedForce = select.value();
  
  if (selectedForce === 'Resistencia de fluidos') {
    liquid.show();
  }
  
  if (selectedForce === 'Atracción gravitacional') {
    attractor.display();
    if (attracting) {
      let attraction = attractor.attract(mover);
      mover.applyForce(attraction);
    }
  }
  
  let gravity = createVector(0, 0.2 * mover.mass);
  mover.applyForce(gravity);
  
  if (selectedForce === 'Fricción' && mover.velocity.mag() > 0) {
    let friction = mover.calculateFriction(0.1);
    mover.applyForce(friction);
  }
  
  if (selectedForce === 'Resistencia del aire') {
    let airResistance = mover.calculateAirResistance(0.02);
    mover.applyForce(airResistance);
  }
  
  if (selectedForce === 'Resistencia de fluidos' && liquid.contains(mover)) {
    let fluidResistance = liquid.drag(mover);
    mover.applyForce(fluidResistance);
  }
  
  mover.update();
  mover.edges();
  mover.display();
}

function resetMover() {
  mover = new Mover(50, 200, 2);
}

function mousePressed() {
  if (select.value() === 'Atracción gravitacional') {
    attracting = true;
  }
}

function mouseReleased() {
  attracting = false;
}

class Mover {
  constructor(x, y, m) {
    this.position = createVector(x, y);
    this.velocity = createVector(2, 0);
    this.acceleration = createVector(0, 0);
    this.mass = m;
  }
  
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }
  
  calculateFriction(mu) {
    let friction = this.velocity.copy();
    friction.normalize();
    friction.mult(-1);
    let normalForce = this.mass * 0.2;
    friction.setMag(mu * normalForce);
    return friction;
  }
  
  calculateAirResistance(c) {
    let airResistance = this.velocity.copy();
    airResistance.normalize();
    airResistance.mult(-1);
    let speedSq = this.velocity.magSq();
    airResistance.setMag(c * speedSq);
    return airResistance;
  }
  
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }
  
  edges() {
  if (this.position.y >= height - 10) {
    this.position.y = height - 10;
    this.velocity.y *= -0.5;
  }
  if (this.position.x <= 0) {
    this.position.x = 0;
    this.velocity.x *= -0.5;
  }
  if (this.position.x >= width) {
    this.position.x = width;
    this.velocity.x *= -0.5;
  }
  if (this.position.y <= 0) {
    this.position.y = 0;
    this.velocity.y *= -0.5;
  }

  }
  
  display() {
    fill(0);
    ellipse(this.position.x, this.position.y, this.mass * 10, this.mass * 10);
  }
}

class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;
  }
  
  contains(m) {
    return m.position.y > this.y;
  }
  
  drag(m) {
    let speed = m.velocity.mag();
    let dragMagnitude = this.c * speed * speed;
    let dragForce = m.velocity.copy().mult(-1).normalize().mult(dragMagnitude);
    return dragForce;
  }
  
  show() {
    fill(0, 0, 255, 100);
    noStroke();
    rect(this.x, this.y, this.w, this.h);
  }
}

class Attractor {
  constructor(x, y, m) {
    this.position = createVector(x, y);
    this.mass = m;
  }
  
  attract(m) {
    let force = p5.Vector.sub(this.position, m.position);
    let distance = force.mag();
    
    // Evitar una distancia muy pequeña para evitar una fuerza infinita
    distance = constrain(distance, 5, 25);
    
    let strength = (0.4 * this.mass * m.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }
  
  display() {
    fill(255, 0, 0);
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
}
```

**Explicación de la aplicación de fuerzas**

**1. Gravedad:** se modeló como una fuerza constante dirigida hacia abajo. Su magnitud es proporcional a la masa del objeto, siguiendo la ecuación: Fg= m*g y asegurando que el objeto experimente una aceleración hacia abajo proporcional a su masa.

En el código, esto se representa mediante un vector de fuerza aplicado en cada iteración del ciclo draw():

```js
let gravity = createVector(0, 0.2 * mover.mass);
mover.applyForce(gravity);
```


**2. Fricción:** se modeló como una fuerza opuesta a la dirección del movimiento. Su magnitud depende de la fuerza normal y el coeficiente de fricción: Fr= μ * N, en este caso la fuerza normal es el peso del objeto, por lo que se aproxima a N= m * g

```js
calculateFriction(mu) {
  let friction = this.velocity.copy();
  friction.normalize();
  friction.mult(-1);
  let normalForce = this.mass * 0.2;
  friction.setMag(mu * normalForce);
  return friction;
}
```

Se asegura que la fricción tenga la dirección opuesta a la velocidad del objeto y se aplique solo si el objeto se está moviendo.

**3.1 Resistencia del aire:** se consideró que su magnitud depende del cuadrado de la velocidad del objeto-> Fa= c * v^2

```js
calculateFriction(mu) {
  let friction = this.velocity.copy();
  friction.normalize();
  friction.mult(-1);
  let normalForce = this.mass * 0.2;
  friction.setMag(mu * normalForce);
  return friction;
}
```
Se normaliza la dirección de la velocidad para invertirla y se ajusta la magnitud de la fuerza en función del coeficiente c y el cuadrado de la velocidad.

**3.2 Resistencia de fluidos:** Esta fuerza actúa solo cuando el objeto está dentro de un área de fluido y se basa en la misma fórmula que la resistencia del aire--> 
F = c * v^2. Se verifica si el objeto está dentro del fluido y luego se aplica la fuerza de arrastre


El código verifica si el objeto está dentro del fluido y luego aplica la fuerza de arrastre:

```js
if (liquid.contains(mover)) {
  let fluidResistance = liquid.drag(mover);
  mover.applyForce(fluidResistance);
}
```
Luego se calcula la fuerza dentro de la función drag ()

```js
drag(m) {
  let speed = m.velocity.mag();
  let dragMagnitude = this.c * speed * speed;
  let dragForce = m.velocity.copy().mult(-1).normalize().mult(dragMagnitude);
  return dragForce;
}
```

**4. Atracción gravitacional:** para modelar la atracción gravitacional entre el objeto y un punto específico (Attractor), 
se usó la ecuación de la ley de gravitación universal de Newton. Se evita que la distancia 
sea demasiado pequeña para prevenir fuerzas excesivamente grandes. 

La dirección de la fuerza apunta hacia el attractor, creando una atracción que se activa cuando se presiona el mouse.

```js
attract(m) {
  let force = p5.Vector.sub(this.position, m.position);
  let distance = force.mag();
  distance = constrain(distance, 5, 25);
  let strength = (0.4 * this.mass * m.mass) / (distance * distance);
  force.setMag(strength);
  return force;
}
```


 
