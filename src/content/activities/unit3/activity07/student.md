**Planteamiento inicial**

¿Qué pasa si en un frame actúan sobre un objeto dos fuerzas? ¿Cómo calculas la aceleración resultante?
```js
mover.applyForce(wind);
mover.applyForce(gravity);
----------------
applyForce(force) {
    // Asume que la masa es 10
    force.div(10);
    this.acceleration.add(force);
}
```
> **¿Qué problema le ves a este planteamiento y qué solución propones?**

* El problema en este planteamiento es que force, al ser un objeto de la clase p5.Vector, se pasa por referencia en la función **applyForce()**.

> Esto significa que cualquier modificación dentro de la función afectará el objeto original fuera de ella.

En este caso, al dividir force por 10 en la primera llamada, el cambio también impacta en la siguiente fuerza aplicada, 
lo que genera un resultado incorrecto en la simulación.
Lo que realmente queremos es modificar la fuerza solo dentro de la función, sin alterar su valor original. La solución es crear una copia del vector antes de hacer cualquier operación, 
usando **force.copy()**, para asegurarnos de que cada fuerza se aplique de manera independiente. Así, en lugar de modificar directamente el argumento recibido, trabajamos con una copia que 
se ajusta y luego se suma a la aceleración. De esta manera, cada llamada a **applyForce()** procesa la fuerza correctamente sin afectar las demás, asegurando una simulación más precisa y 
coherente en p5.js.

> **¿Cómo lo implementarías en p5.js?**
```js
applyForce(force) {
    let f = force.copy(); // Crear una copia del vector para no modificar el original
    f.div(10); // Dividir por la masa
    this.acceleration.add(f); // Sumar a la aceleración
} 
```

