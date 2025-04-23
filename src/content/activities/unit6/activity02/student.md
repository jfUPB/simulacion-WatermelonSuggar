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

Ese vector le dice hacia dónde debería ir y qué tan rápido. Entonces, el agente:

1. Toma ese vector y lo ajusta a su velocidad máxima, porque quiere ir hacia allá a toda velocidad.
2. Compara esa nueva dirección con la velocidad que ya lleva. La diferencia entre ambas le dice cuánto necesita girar o cambiar su movimiento.
3. Esa diferencia es la fuerza de dirección (steering force).
4. Para no hacer giros bruscos, limita esa fuerza a un valor máximo.
5. Finalmente, aplica esa fuerza para corregir su rumbo suavemente.



