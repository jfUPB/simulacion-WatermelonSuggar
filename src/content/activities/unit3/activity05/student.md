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

> **¿Qué solución propones?** 

> **¿Cómo lo implementarías en p5.js?**

Nota: no olvides que queremos calcular la aceleración en cada frame como la sumatoria de todas las fuerzas que actúan sobre un objeto.
