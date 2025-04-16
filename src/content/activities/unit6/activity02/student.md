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

* 

🌊 Una vez que tiene el vector deseado del campo, ¿cómo lo utiliza para calcular la fuerza de dirección (steering force)? (pista: implica calcular la diferencia con la velocidad actual y limitar la fuerza).

