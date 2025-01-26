![ruidoPerlin](../../../..//assets/ruidoPerlinVsRandomNoise.png)

La imagen de la izquierda, que representa el ruido de Perlín, muestra un gráfico más agradable a la vista, con variaciones ligeras entre sus trazos que generan una sensación de coherencia, fluidez y suavidad, por otro lado,
la imagen de la derecha representa un gráfico generado con números aleatorios, que varían con mayor frecuencia creando esta sensación de acumulación de información en la que los datos varían drásticamente entre ellos.

🌻**Jardín interactivo**

Empleé el ruido de Perlin para crear un jardín interactivo. Lo primero que se presenta es un canvas con algunos insectos revoloteando, la posición de cada insecto se calcula a partir de dos valores de ruido (uno para cada eje), noise(this.noiseX) y noise(this.noiseY) que generan valores entre 0 y 1 que son escalados al ancho y alto del lienzo, respectivamente. Esto produce un movimiento continuo y sin saltos, imitando el comportamiento errático pero fluido de un insecto real.

Al hacer click sobre el canvas se dibujan en él algunas flores, el ruido de Perlin ayuda en la generación de características visuales únicas para cada flor como el color de sus pétalos, el tamaño y hasta la cantidad de pétalos, características que no varían tanto debido a los pequeños saltos generados por el mismo ruido.


**Flores:** Cada flor tiene características únicas pero coherentes, simulando diversidad natural en colores y formas.

**Insectos:** Los insectos se mueven de manera fluida y aleatoria, replicando comportamientos biológicos observados en la naturaleza.
