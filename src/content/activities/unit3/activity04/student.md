**Recordando el Marco Motion 101**

> En el código de ejemplo: ¿Dónde está el marco motion 101?

En la función Update() puedo identificar un patrón característico del Motion 101, en donde la aceleración afecta la velocidad y esta a su vez modifica la posición.

```js
update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
}
```

> En la unidad anterior tu definías la aceleración mediante algún algoritmo. ¿Cuáles eran? Muestra ejemplos de código de la unidad anterior (solo la parte donde se define la aceleración).

**Aceleración constante**
```js
  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
  }
```
**Aceleración aleatoria**

```js
update() {
  this.acceleration = p5.Vector.random2D();
  this.velocity.add(this.acceleration);
  this.velocity.limit(this.topSpeed);
  this.position.add(this.velocity);
}
```
**Aceleración dependiendo de la posición del mouse**

```js
  update() {
    let mouse = createVector(mouseX, mouseY);
    // Step 1: Compute the direction.
    let dir = p5.Vector.sub(mouse, this.position);
    // Step 2: Normalize.
    dir.normalize();
    // Step 3: Scale.
    dir.mult(0.2);
    //{!1} Step 4: Accelerate.
    this.acceleration = dir;
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
  }
```

> En esta unidad tu vas a calcular la aceleración. ¿Qué tiene que ver esto con las leyes de movimiento de Newton?

En la unidad anterior definiamos la aceleración de manera arbitraría per ahora debemos aplicar fuerzas al objeto en movimiento basándonos en la segunda ley de Newton.

F= m*a---> a=F/m

**¿Cómo se aplicaría en código?**

```js
applyForce(force) {
    let f = p5.Vector.div(force, this.mass); // a = F/m
    this.acceleration.add(f);
}
```
* **applyForce(force)**--> Convierte una fuerza en una aceleración dividiéndola por la masa del objeto.

* Cada fuerza aplicada afectará la aceleración, que a su vez modificará la velocidad y la posición, permitiendo simulaciones más realistas.
