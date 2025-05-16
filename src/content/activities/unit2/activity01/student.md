### Suma de vectores
____________________________________________________________________________________________________

**Código para analizar**

```js
// Instead of a bunch of floats, you now have just two variables.
let position;
let velocity;

function setup() {
  createCanvas(640, 240);
  // Note that createVector() has to be called inside setup().
  position = createVector(100, 100);
  velocity = createVector(2.5, 2);
}

function draw() {
  background(255);
  position.add(velocity);

  // You still sometimes need to refer to the individual components of a p5.Vector and can do so using the dot syntax: position.x, velocity.y, and so forth.
  if (position.x > width || position.x < 0) {
    velocity.x = velocity.x * -1;
  }
  if (position.y > height || position.y < 0) {
    velocity.y = velocity.y * -1;
  }

  stroke(0);
  fill(127);
  circle(position.x, position.y, 48);
}

```

**¿Cómo funciona la suma dos vectores?**

Ocurre justo en la función draw() del código

```js
position.add(velocity);
```

* position es un vector con coordenadas (x,y)
* Velocity es otro vector con coordenadas (vx, vy)

La suma de vectores se hace sumando cada componente individualmente (x + vx, y + vy).

El método **.add()** en p5.Vector suma cada componente del vector actual con las componentes de otro vector.

"Toma el vector position y agrégale el vector velocity, sumando x con vx y y con vy."

xnuevo = xactual + vx

ynuevo = yactual + vy


> Internamente lo que hace .add() es:
> ```js
>  position.x = position.x + velocity.x;
>  position.y = position.y + velocity.y;
> ```


¿Por qué esta línea position = position + velocity; no funciona?

* Esta línea no funcionaría porque "position" y "velocity" son objetos tipo p5.vestor por lo que en Javascript no se pueden sumanr objetos directamente con el +.

¿Cómo se hace correctamente?

1. Usar el método **.add()** para modificar el vector original (como en el código analizado)
2. Usar **p5.Vector.add()** crea un nuevo objeto (pero usas más espacio en la memoria)

```js

let newposition = p5.Vector.add(position, velocity);

```




