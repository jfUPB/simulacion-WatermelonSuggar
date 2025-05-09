> Identifica motion 101. ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas? Trata de recordar por qué es necesario hacer esta modificación.

* En el código, esto está implementado en la clase **Mover** en el método **update()**.

El marco de Motion 101 describe cómo los objetos se mueven en un entorno simulado utilizando posición, velocidad y aceleración.

* Aplicar una fuerza → Se suma una aceleración basada en la fuerza aplicada.
* Actualizar la velocidad → La aceleración cambia la velocidad.
* Actualizar la posición → La velocidad cambia la posición.

⭐Cuando se aplican múltiples fuerzas en un solo frame, es importante sumarlas antes de actualizar la velocidad y posición. En **applyForce()**, las fuerzas se dividen por la masa y se agregan a la aceleración:

```js
applyForce(force) {
  let f = p5.Vector.div(force, this.mass);
  this.acceleration.add(f); // Se acumulan todas las fuerzas aplicadas en el frame
}
```
Luego, en **update()**, la aceleración se usa para modificar la velocidad y la posición:
```js
update() {
  this.velocity.add(this.acceleration); 
  this.position.add(this.velocity); 
  this.acceleration.mult(0); // Importante: Se resetea la aceleración para el siguiente frame
}
```
⭐Si no lo hacemos, las fuerzas se seguirían acumulando indefinidamente, lo que haría que el objeto acelerara de forma incontrolable. Al resetearla en cada frame, solo se consideran las fuerzas aplicadas en ese instante.

> Identifica dónde está el Attractor en la simulación. Cambia el color de este.

* Se dibuja en la clase attractor.js y se dibuja en el método **display()**

```js
display() {
  ellipseMode(CENTER);
  stroke(0);
  if (this.dragging) {
    fill(50);
  } else if (this.rollover) {
    fill(100);
  } else {
    fill(161, 82, 255); // Aquí le cambié el color por moradooo
  }
  ellipse(this.position.x, this.position.y, this.mass * 2);
}
```

> Observa que el Attractor tiene dos atributos this.dragging y this.rollover. Estos atributos no se modifican en el código, pero permitirían mover el attractor con el mouse y cambiar su color cuando el mouse está sobre él. ¿Cómo podrías modificar el código para que esto funcione? considera las funciones que ofrece p5.js para interactuar con el mouse.

[Simulación modificada](https://editor.p5js.org/WatermelonSuggar/sketches/FYfz45Lc3)

* **Movimiento del Attractor con el mouse:** implementé **mousePressed()** y **mouseReleased()** para arrastrar el Attractor.
* **Cambio de color cuando el mouse está sobre el Attractor:** agregué una función **rollover()** que detecta si el mouse está sobre el Attractor y cambia su color por fucsia.



