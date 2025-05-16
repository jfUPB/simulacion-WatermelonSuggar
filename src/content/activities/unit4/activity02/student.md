**Simulación 1** 

[Manejo de ángulos](https://torrentio.strem.fun/manifest.json)

> ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?

  En este código, se está creando una animación en la que una línea y dos círculos rotan alrededor de su punto central. 
La interacción se da cuando presionas una tecla, ya que al presionar cualquier tecla, la variable angle aumenta en 0.1, lo que hace que los elementos gráficos rotan.
  
> Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?

  La instrucción **translate(width / 2, height / 2)**; mueve el origen del sistema de coordenadas al centro de la pantalla. Es decir, quepara realizar el desplazamiento no se hace manualmente sino que se realiza una rotación en el centro del canvas. Cuando el origen está en el centro, todo lo que se dibuje (líneas, círculos, etc.) se rotará alrededor de este centro.

> Cuál es la relación entre el sistema de coordenadas y la función rotate().

  La función **rotate()** rota los objetos alrededor del origen del sistema de coordenadas. Al mover el origen con **translate()**, la rotación afectará a los objetos en relación con el nuevo centro (que ahora está en el centro de la pantalla). Sin translate(), la rotación se aplicaría respecto al origen por defecto (en la esquina superior izquierda).

> Observa que al dibujar los elementos gráficos parece que se está dibujando en la posición (0, 0) del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

Los elementos se dibujan con coordenadas relativas al origen (0,0), pero como translate(width / 2, height / 2) mueve el origen al centro de la pantalla, todo se posiciona alrededor de ese punto. Aunque el código de dibujo es el mismo en cada frame, rotate(angle) aplica una rotación acumulativa. Como angle aumenta con cada tecla presionada, los objetos giran progresivamente en torno al centro.

**Simulación 2**

[Apunten en la dirección del movimiento](https://editor.p5js.org/natureofcode/sketches/bZqHGYbRQ)

> Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?

El código aplica Motion 101 en la función **update()**, donde en cada frame: se calcula la dirección y la aceleración, se ajusta la velocidad y se actualiza la posición.

> Observa detenidamente este fragmento de código de la simulación:

```js
  display() {
    let angle = this.velocity.heading();

    stroke(0);
    strokeWeight(2);
    fill(127);
    push();
    rectMode(CENTER);
    translate(this.position.x, this.position.y);
    rotate(angle);
    rect(0, 0, 30, 10);

    pop();
  }
```

> ¿Qué hace la función heading()?

Es un método de p5.Vector que devuelve el ángulo de dirección de un vector respecto al eje x.

```js
let angle = this.velocity.heading();
```

**this.velocity.heading()** obtiene el ángulo en radianes que forma el vector de velocidad con el eje x. Este ángulo se usa para rotar el objeto en la dirección en la que se mueve.

> ¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.

push(): Guarda el estado actual del sistema de coordenadas.
pop(): Restaura el estado guardado antes del push().

Sin push() y pop(), 
Con push() y pop(), las transformaciones solo afectan el rectángulo del objeto y no el resto del lienzo.

Sin push() y pop() (Transformaciones Acumuladas): [experimento 1](https://editor.p5js.org/WatermelonSuggar/sketches/43ussRnP9)

* Cada frame, el sistema de coordenadas se sigue rotando y trasladando.
* El rectángulo parece girar de forma descontrolada.

Con push() y pop() (Transformaciones Controladas): [experimento 2](https://editor.p5js.org/WatermelonSuggar/sketches/3b9SN0N0d)

* El rectángulo azul en el centro rota correctamente.
* El rectángulo rojo no se ve afectado por la rotación, porque está fuera de push() y pop().

Varias transformaciones independientes: [experimento 3](https://editor.p5js.org/WatermelonSuggar/sketches/JSVx13Btd)

> ¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.

Esta función cambia la forma en que se posicionan los rectángulos. 

* Por defecto **(rectMode(CORNER))**, un rectángulo se dibuja desde la esquina superior izquierda.
* Con **rectMode(CENTER)**, el rectángulo se dibuja desde su centro.

[experimento](https://editor.p5js.org/WatermelonSuggar/sketches/MoAMZx7Ra)

> ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad?

* El vector de velocidad (this.velocity) define la dirección y rapidez con la que se mueve el objeto.
* El ángulo de rotación (angle) se obtiene con this.velocity.heading(), que devuelve el ángulo en radianes correspondiente a la dirección del vector de velocidad.
* La rotación del objeto (rotate(angle)) hace que el dibujo del objeto se alinee con la dirección en la que se mueve.


