## Análisis de los enjambres (Flocking)

### Las 3 reglas

**Identificación**

* Se encuentran en la clase boid.js y se implementan cada una como un método de esta clase

> Separación

```js

  // Separation
  // Method checks for nearby boids and steers away
  separate(boids) {
    let desiredSeparation = 25;
    let steer = createVector(0, 0);
    let count = 0;
    // For every boid in the system, check if it's too close
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      // If the distance is greater than 0 and less than an arbitrary amount (0 when you are yourself)
      if (d > 0 && d < desiredSeparation) {
        // Calculate vector pointing away from neighbor
        let diff = p5.Vector.sub(this.position, boids[i].position);
        diff.normalize();
        diff.div(d); // Weight by distance
        steer.add(diff);
        count++; // Keep track of how many
      }
    }
    // Average -- divide by how many
    if (count > 0) {
      steer.div(count);
    }

    // As long as the vector is greater than 0
    if (steer.mag() > 0) {
      // Implement Reynolds: Steering = Desired - Velocity
      steer.normalize();
      steer.mult(this.maxspeed);
      steer.sub(this.velocity);
      steer.limit(this.maxforce);
    }
    return steer;
  }
```

> Alineación

```js
// Alignment
  // For every nearby boid in the system, calculate the average velocity
  align(boids) {
    let neighborDistance = 50;
    let sum = createVector(0, 0);
    let count = 0;
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      if (d > 0 && d < neighborDistance) {
        sum.add(boids[i].velocity);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      sum.normalize();
      sum.mult(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.velocity);
      steer.limit(this.maxforce);
      return steer;
    } else {
      return createVector(0, 0);
    }
  }
```

> Cohesión

```js
  // Cohesion
  // For the average location (i.e. center) of all nearby boids, calculate steering vector towards that location
  cohere(boids) {
    let neighborDistance = 50;
    let sum = createVector(0, 0); // Start with empty vector to accumulate all locations
    let count = 0;
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      if (d > 0 && d < neighborDistance) {
        sum.add(boids[i].position); // Add location
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      return this.seek(sum); // Steer towards the location
    } else {
      return createVector(0, 0);
    }
  }
```
_________________________________________________________________________________________________

**Explicación**

**1. Separación:** evita colisiones o que los boids estén demasiado juntos. 
1. Revisa los demás boids a su alrededor (dentro de un cierto radio).
2. Si encuentra a otros boids muy cerca (más cerca que desiredSeparation), calcula un vector en dirección contraria para alejarse.
3. Si hay varios boids cerca, hace un promedio de esas direcciones.



**2. Alineación:** las partículas se mueven en la misma dirección promedio que sus vecinos.

1. Encuentra los boids cercanos dentro de un radio (neighborDistance).
2. Promedia las velocidades (dirección + rapidez) de sus vecinos cercanos.
3. Intenta ajustar su velocidad para que coincida con ese promedio.

**3. Cohesión:** evita que el grupo se disperse haciendo que se acerquen al centro del grupo cercano.

1. Identifica a los vecinos cercanos dentro de un radio (neighborDistance).
2. Calcula el centro de masa (posición promedio) de los boids cercanos.
3. Calcula un vector hacia ese punto central.
4. Usa seek(target) para calcular una fuerza que lo lleve hacia ese centro.

La variable clave es: 

```js
let neighborDistance = 50; // distancia para considerar vecinos
```

> Igual que la alineación.

### Parámetros clave

**El radio o distancia de percepción** define qué tan lejos puede ver un boid para aplicar cada regla.

* La variable clave en **separación** es:

```js
let desiredSeparation = 25; // qué tan cerca es “demasiado cerca”
```

  * Las reglas de **alineación y cohesión** comparten la variable clave: 

```js
let neighborDistance = 50; // distancia para considerar vecinos
```
___________________________________________________________________________________________________________

Los **pesos o influencias relativas** se aplican dentro del método flock(boids) de la clase boid.js justo antes de sumar las fuerzas de aceleración.

```js
  flock(boids) {
    let sep = this.separate(boids); // Separation
    let ali = this.align(boids);    // Alignment
    let coh = this.cohere(boids);   // Cohesion

    // 🟢 Aquí están los multiplicadores:
    sep.mult(1.5);  // Separación más fuerte
    ali.mult(1.0);  // Alineación normal
    coh.mult(1.0);  // Cohesión normal

    // Se suman todas las fuerzas a la aceleración
    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);
  }
```

> Es aquí donde se otorga una jerarquía de importancia para cada regla, en este caso la que tiene prioridad es la de separación (para evitar colisisones).

_____________________________________________________________________________________________________________________

La **velocidad máxima** limita la velocidad en la que se puede mover un boid y la **fuerza máxima** limita la velocidad con la que cambia de dirección (si es suave o brusco)

> En el código se encuentra en el constructor del boid
```js
this.maxspeed = 3;   // ← Velocidad máxima
this.maxforce = 0.05; // ← Fuerza de dirección máxima (steering)
```
____________________________________________________________________________________________

### Experimentación

![image](https://github.com/user-attachments/assets/81de7795-1f57-4e39-996f-2de6922c37d1)

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/wjAXfCcWb)}

**Modificaciones**

1. Cambié drásticamente el peso de la separación, pasó de 1.5 a 5.0 pero también agregué otra fuerza de target que es la que atrae a los boids y es la principal con un peso de 5.5. La cohesión y alineación quedaron iguales. En la simulación se nota cómo se prioriza seguir al nuevo objeto y también cómo se mantienen separados, dispersos y salen desde varias direcciones.
   
2. Modifiqué el radio de separación por 90 así que los boids van bastante separados, quería ver qué pasaba si las variables claves de la cohesión y la alineación eran diferentes, así que con la primera conservé el valor de 50 y a la segunda uno de 20.

* Ahora los boids tienden a mantenerse mucho más separados entre sí, como si se repelieran fuertemente, incluso desde la distancia.
  
* Manteniendo la cohesión sigue igual la atracción hacia el centro del grupo, por lo que los boids podrían intentan agruparse constantemente, luchando contra la separación.

* Con la alineación los boids copien rápidamente la dirección del grupo, incluso cuando están bastante lejos entre sí.
  
3. Introduje un objetivo (target) que todos los boids intentan seguir (usando una fuerza de seek) y es la nueva fuerza principal del ejemplo por lo que siempre tienden a donde se encuentra el target que es justo la posición del mouse sobre el canvas.


