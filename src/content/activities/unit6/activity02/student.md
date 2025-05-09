# Análisis de los flowFields

## Preguntas

### La estructura del campo

☀️¿Qué estructura de datos se usa?

* Se está usando un array bidimensional. Una matriz con tamaño de columnas x filas y cada elemento ue almacena es un vector d etipo p5.vector.

☀️¿Qué representa cada elemento de esa estructura?

* Cada celda field[i][j] contiene un vector unitario (p5.Vector) que indica una dirección.

☀️¿Cómo se calcula inicialmente el vector en cada punto?

* En el código se calcula gracias al uso del ruido de Perlin. Específicamente así:
  * noise(xoff, yoff) genera un valor suave entre 0 y 1.
  * Se mapea ese valor a un ángulo entre 0 y 2π.
  * Se convierte ese ángulo a un vector unitario (dirección).
  * Finalmente, cada punto tiene una dirección fluida, con continuidad espacial gracias al ruido de Perlin.
 
### Comportamiento del agente/vehículo

🌊 ¿Cómo determina el agente qué vector del campo de flujo debe seguir basándose en su posición actual? (pista: implica mapear la posición a índices de la cuadrícula).

* El agente está en una posición dentro del lienzo (por ejemplo, en (x, y)), pero el campo de flujo está organizado como una cuadrícula, como si dividieras el espacio en casillas.Entonces, lo primero que hace es ver en qué casilla está, dividiendo su posición entre el tamaño de las casillas (la "resolución").

Así convierte su posición en coordenadas de fila y columna dentro del campo, y con eso busca el vector que le corresponde en ese punto.

🌊 Una vez que tiene el vector deseado del campo, ¿cómo lo utiliza para calcular la fuerza de dirección (steering force)? (pista: implica calcular la diferencia con la velocidad actual y limitar la fuerza).

* Ese vector le dice hacia dónde debería ir y qué tan rápido. Entonces, el agente:

1. Toma ese vector y lo ajusta a su velocidad máxima, porque quiere ir hacia allá a toda velocidad.
2. Compara esa nueva dirección con la velocidad que ya lleva. La diferencia entre ambas le dice cuánto necesita girar o cambiar su movimiento.
3. Esa diferencia es la fuerza de dirección (steering force).
4. Para no hacer giros bruscos, limita esa fuerza a un valor máximo.
5. Finalmente, aplica esa fuerza para corregir su rumbo suavemente.

## Parámetros clave

🪸La resolución del campo de flujo (el tamaño de las celdas de la cuadrícula).

* Esto controla qué tan grandes son las celdas de la cuadrícula del campo de flujo (es decir, cuántas flechas habrá en el lienzo).
* En el código se encuentra en sketch.js

```js
flowfield = new FlowField(20);
```

> El número 20 es la resolución. Mientras más bajo el número, más celdas y más detalles tendrá el campo (porque cada celda será más pequeña).

🪸La velocidad máxima (maxspeed) y la fuerza máxima (maxforce) de los agentes.

* **Velocidad máxima:** es el parámetro que limita qué tan rápido puede ir el agente otorgando un tope máximo evitando que se descontrole. Se define en la siguiente patrte de sketch.js

* **Fuerza máxima:** es quien controla cuánto puede cambiar de dirección un agente en cada momento. Si es muy baja, el giro será suave. Si es alta, puede cambiar de rumbo más bruscamente.

```js
new Vehicle(random(width), random(height), random(2, 5), random(0.1, 0.5))
```

>  El tercer parámetro (random(2, 5)) es la velocidad máxima, y varía aleatoriamente entre 2 y 5 para cada agente. 

> El cuarto parámetro (random(0.1, 0.5)) es la fuerza máxima de dirección, significa que cada agente tendrá una fuerza de giro diferente entre 0.1 y 0.5.

## Experimentación

![image](https://github.com/user-attachments/assets/c76ae783-14f8-49f8-b4ac-354f364bb604)

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/bRtYgLCUZ)

**Modificaciones aplicadas**

1. Los vectores ahora se generan de manera diferente, ahora siguen una forma en espiral. La dirección de cada vector se calcula como un ángulo basado en la diferencia de posición (dir.heading()) y la magnitud (distancia al centro) multiplicada por un factor (dir.mag() * 0.05). Este factor asegura que los vectores se van girando progresivamente a medida que aumentan su distancia al centro, creando así una trayectoria en espiral.
2. Reduje la resolución en un 50% por lo que ahora hay más celdas o cuadrículas para que los vehículos direccionen su movimiento.
3. Asigné una velocidad máxima de 1, así que los agentes se mueven MUCHO más lento tratando en identificar la celda en la que están para poder moverse y una fuerza máxima de 0.02 por lo que gira lentamente y ahora parecen agentes perezosos.

