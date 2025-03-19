**Simulación con un oscilador de objetos**

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/qjoUbtUed9)


**Activación del modo atracción**

Cuando la fuerza de atracción está activada (es decir, el botón es verde), los osciladores sienten una fuerza de atracción hacia el centro de la pantalla. 
Esta fuerza se calcula como la diferencia entre la posición del centro de la pantalla y la posición de cada oscilador.

> Esta diferencia es muy pequeña lo que ocasiona que la fuerza de ocasión no sea tan potente y las partículas "se congelen" en su posición.
![image](https://github.com/user-attachments/assets/7882b5c7-978f-4d02-8e28-7048aa7890fc)


**Modo desactivado - locura total**

![image](https://github.com/user-attachments/assets/d91724b3-15ba-4fae-9131-6b209874ce1c)

**Estado:** Cuando el botón es rojo (modo attractMode = false).

**Comportamiento:** Los osciladores no son atraídos hacia el centro de la pantalla. En lugar de eso, siguen su camino aleatorio influenciado por las fuerzas de aceleración aleatoria y las fluctuaciones de amplitud.

> Los osciladores experimentan un movimiento caótico debido a las fuerzas aleatorias que alteran su velocidad y aceleración en cada fotograma.
Los rebotes elásticos siguen ocurriendo cuando los osciladores alcanzan los bordes de la pantalla.
