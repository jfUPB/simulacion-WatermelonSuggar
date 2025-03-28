**Aplicaciones de Motion 101**

escribe cómo los conceptos de esta unidad pueden ser utilizados en el diseño de videojuegos, experiencias interactivas o animaciones. Da ejemplos concretos.

**VIDEOJUEGOS**

  * En juegos como Hollow Knight o Celeste, el personaje no cambia su velocidad de inmediato cuando el jugador presiona un botón de dirección. En su lugar, la velocidad aumenta progresivamente debido a la aceleración.

Aplicación del Motion 101: 

```js
velocity += acceleration;  // El personaje aumenta de velocidad gradualmente
position += velocity;      // Se actualiza la posición del personaje

```

* Cuando el jugador presiona izquierda o derecha, la velocidad del personaje aumenta gradualmente en lugar de hacerlo instantáneamente.
* Al soltar la tecla, la aceleración negativa hace que el personaje frene suavemente en lugar de detenerse bruscamente.
* Al saltar, la gravedad afecta la aceleración de manera fluida, creando un arco natural en la caída.

**EXPERIENCIAS INTERACTIVAS**
  * Imagina una experiencia interactiva donde los botones o elementos de la interfaz no reaccionan de manera rígida al clic del usuario, sino que se estiran y rebotan suavemente, como si estuvieran hechos de un material flexible.

Aplicación del Motion 101: 

```js
let stretch = 0;
let velocity = 0;
let acceleration = 0;
let damping = 0.9; // Reduce el movimiento con el tiempo

function onClick() {
    acceleration = random(0.5, 1.5); // Variación de fuerza según el clic
}

function update() {
    velocity += acceleration;
    stretch += velocity;
    velocity *= damping; // Simulación de amortiguación
}

```

* Cada vez que un usuario hace clic en un botón, este se deforma brevemente y luego regresa a su forma original con una oscilación amortiguada.
* La aceleración y la velocidad del rebote dependen de la intensidad del clic (si el usuario mantiene presionado, el rebote es más fuerte).
* Se pueden aplicar fuerzas de fricción para evitar oscilaciones infinitas y hacer que la interfaz se sienta más realista.


**ANIMACIÓN**
  * Motion 101 es clave para simular movimientos celestes realistas, como el de planetas, lunas o asteroides alrededor de un cuerpo masivo en el espacio. En lugar de usar trayectorias predefinidas, podemos aplicar aceleración y velocidad para generar órbitas dinámicas y realistas.


Aplicación del Motion 101: 

```js
let planet = { x: 200, y: 100, vx: 2, vy: 0 };
let sun = { x: 300, y: 300, mass: 10000 };

function update() {
    let dx = sun.x - planet.x;
    let dy = sun.y - planet.y;
    let distance = sqrt(dx * dx + dy * dy);
    let force = sun.mass / (distance * distance); // Ley de gravitación

    let ax = force * (dx / distance);
    let ay = force * (dy / distance);

    planet.vx += ax;
    planet.vy += ay;
    planet.x += planet.vx;
    planet.y += planet.vy;
}

```

* Cada planeta tiene una velocidad inicial y es afectado por la gravedad del sol (aceleración dirigida hacia el centro).
* Se usa la ley de la gravitación de Newton para calcular la atracción entre cuerpos en tiempo real.
* Si el usuario lanza un planeta con poca velocidad, este caerá al sol. Si lo hace con suficiente velocidad tangencial, entrará en órbita.
* Se puede agregar fricción atmosférica si el planeta pasa cerca de una estrella, alterando su trayectoria.
