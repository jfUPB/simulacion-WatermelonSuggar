**Planteamiento inicial:**
```js
class Mover {
  constructor() {
    this.position = createVector();
    this.velocity = createVector();
    this.acceleration = createVector();
  }
  
  update() {
      mover.applyForce(wind);
      mover.applyForce(gravity);
    }
    
  applyForce(force) {
    this.acceleration = force;
  }

}

```

> **¿Qué problema le ves a este planteamiento?**

* En el método **applyForce(force)** la aceleración se está sobreescribiendo en cada llamada en lugar de acumularse con cada fuerza que se aplica sobre el objeto. Es decir, cuando llamamos a la fuerza del viento y después a la fuerza de gravedad, esta última reemplaza a la primera en lugar de sumarse a ella y es por ello que se elimina "el efecto de viento".


> **¿Qué solución propones?**

* Para evitar que se sobreescriban las fuerzas, es necesario sumar la nueva fuerza a la aceleración acumulada por la anterior, permitiendo que hayan varias fuerzas que influyan en un mismo movimiento.

> **¿Cómo lo implementarías en p5.js?**

```js
applyForce(force) {
  this.acceleration.add(force); // Se suma en lugar de reemplazar
}
```


