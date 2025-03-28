**Nuevo código aplicando el concepto de vectores**

```js
let walkers = [];

function setup() {
  createCanvas(640, 640);
  walkers.push(new Walker(100, 100, color(0, 134, 255)));
  walkers.push(new Walker(540, 540, color(255, 201, 0)));
  walkers.push(new Walker(540, 100, color(255, 251, 0)));
  walkers.push(new Walker(100, 540, color(255, 0, 255)));
  walkers.push(new Walker(100, 350, color(0, 255, 127)));
  walkers.push(new Walker(540, 350, color(229, 36, 36)));
  walkers.push(new Walker(350, 100, color(255, 0, 193)));
  walkers.push(new Walker(350, 540, color(124, 255, 0)));
  background(0);
}

function draw() {
  for (let walker of walkers) {
    walker.step();
    walker.show();
  }
}

class Walker {
  constructor(x, y, col) {
    this.position = createVector(x, y);
    this.color = col || color(0);
    this.reversed = false;
  }

  show() {
    stroke(this.color);
    strokeWeight(5);
    point(this.position.x, this.position.y);
  }

  step() {
    let center = createVector(width / 2, height / 2);
    let direction;

    if (!this.reversed) {
      // Calcular dirección hacia el centro
      direction = p5.Vector.sub(center, this.position);
      direction.setMag(random(1, 3)); // Normalizar y ajustar magnitud
    } else {
      // Movimiento aleatorio después de llegar al centro
      direction = p5.Vector.random2D();
      direction.setMag(2); // Hacer que el movimiento sea constante
    }

    // Comprobar si está lo suficientemente cerca del centro
    if (!this.reversed && this.position.dist(center) < 5) {
      this.reversed = true;
    }

    // Aplicar el movimiento
    this.position.add(direction);
  }
}

```

⭐ Cambios realizados

* **this.position** almacena la posición como un vector en lugar de dos variables separadas.
* **p5.Vector.sub(center, this.position)** obtiene un vector que apunta hacia el centro.
* **.setMag(random(1, 3))** ajusta la magnitud del vector, controlando la velocidad.

* Se reemplazaron los múltiples if para mover x e y manualmente con una sola operación vectorial.
* Se usa **p5.Vector.random2D()** para generar direcciones aleatorias.
* Se creó un array de walkers, eliminando las variables individuales (walker1, walker2, etc.).

> Al usar p5.Vector, p5.js trata la posición y el movimiento como objetos, lo que permite manipularlos con métodos incorporados en lugar de
> manejar manualmente cada componente (x e y).
