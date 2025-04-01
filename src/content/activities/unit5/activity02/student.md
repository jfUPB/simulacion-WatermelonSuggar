**SIMULACIONES A ANALIZAR:**

✅ 1. Revisa detalladamente el ejemplo 4.2: an [Array of Particles](https://natureofcode.com/particles/#example-42-an-array-of-particles).

**Código original**

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

* **Asignación de memoria**

1. Cada vez que creamos una Particle, se reserva espacio en la memoria para su posición, velocidad, aceleración y otros atributos.
2. Si no elimináramos partículas, el array particles crecería infinitamente, causando problemas de rendimiento.

* **Liberación de memoria**
1. Cuando una partícula muere, se elimina del array con splice (i, 1), lo que hace que JavaScript libere su referencia en memoria y permita que el garbage collector la elimine en algún momento. Este enfoque es eficiente porque JavaScript maneja la memoria automáticamente, pero si el número de partículas fuera muy alto, podríamos optimizarlo más.

**Código modificado**

![image](https://github.com/user-attachments/assets/bf789bde-02f1-4354-90bd-2d7b5c9e6f18)

![image](https://github.com/user-attachments/assets/4a0d0951-2e23-4567-9972-ef2786ccd7a6)

[Simulación aquí](https://editor.p5js.org/WatermelonSuggar/sketches/xASgHG0km)

> 🌳Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.

* Hice que las partículas aparezcan y desaparezcan de manera controlada. Con este sistema, evitamos que el programa se vuelva lento por tener demasiadas partículas al mismo tiempo.

* Creación:
  * Cada vez que el programa dibuja un nuevo cuadro, revisa qué tan rápido se está moviendo el mouse.
  * Si el mouse está quieto, aparecen pocas partículas.
  * Si el mouse se mueve rápido, aparecen más.
  * Las partículas se crean justo en la posición del mouse y empiezan a moverse solas.

* Desaparición:
  * Cada partícula tiene un "tiempo de vida" que se va reduciendo con el tiempo.
  * Cuando su tiempo de vida llega a 0, la eliminamos del arreglo que las guarda.
  * Si ya hay muchas partículas, también eliminamos algunas para que la pantalla no se llene demasiado.

> 🌳Explica qué concepto aplicaste, cómo lo aplicaste y por qué.

**Concepto aplicado**
*  Apliqué una **fuerza de atracción inversamente proporcional a la distancia**, es decir que, si la partícula está lejos del mouse, la atracción es más débil. Si la partícula está cerca del mouse, la atracción es más fuerte.

**Cómo lo apliqué**

* Calculé la dirección hacia el mouse, para cada partícula, obtuve un vector que apunta desde su posición hasta el mouse.
* Medí la distancia entre la partícula y el mouse, esto me permite ajustar la intensidad de la atracción.
* Normalicé el vector para que solo indique dirección, así evité que partículas muy lejos reciban una fuerza excesiva.
* Multipliqué la fuerza por un valor inversamente proporcional a la distancia
* Apliqué la fuerza a la partícula, lo que hace que la partícula cambie su aceleración y se mueva hacia el mouse.


**¿Por qué?**

*  Para que las partículas reaccionen al mouse de forma natural en lugar de moverse de manera brusca.
*  Si usara una fuerza constante, todas las partículas se moverían igual sin importar la distancia y haría que la simulación fuera monótona.
*  Para mejorar la visualización de la simulación, dándole una mejor interacción y dinamismo con el mouse.

_________________________________________________________________________________


✅ 2. Analiza el ejemplo 4.4: a [System of Systems.](https://natureofcode.com/particles/#example-44-a-system-of-systems)

**Código original**

> ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

* Creación:
  * Cada vez que se crea un Emisor (Emitter), se inicializa en una posición específica.
  * En cada ciclo de draw(), los emisores generan nuevas partículas en su posición usando addParticle().
  * Estas partículas se agregan a la lista this.particles dentro de cada Emitter.

* Desaparición:
  * Cada partícula tiene una variable _lifespan_ que disminuye en cada frame.
  * Cuando lifespan llega a 0, la partícula se considera "muerta".
  * En run(), se revisa cada partícula con isDead(), y si está muerta, se elimina con splice(i,1). Esto se hace recorriendo el array de atrás hacia adelante (for (let i = this.particles.length - 1; i >= 0; i--)), evitando errores al eliminar elementos dentro de un bucle.

* Gestión de memoria en la simulación
  * Se usa splice(i, 1) para eliminar partículas que han agotado su tiempo de vida. Esto evita acumulación innecesaria de objetos en la memoria, manteniendo el código eficiente.
  * Cada Emitter maneja su propia lista de partículas de forma independiente.
  * No hay eliminación automática de emisores, lo que podría hacer que la simulación consuma más memoria con el tiempo si se crean demasiados.
  * No se almacenan partículas innecesarias.

**Código modificado**

[Simulación aquí](https://editor.p5js.org/WatermelonSuggar/sketches/SgkKsbJCe)

![image](https://github.com/user-attachments/assets/b14fd425-7502-4c00-b33e-dd7da52ac494)


> 🌳Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.

* Creación de partículas:
  * Cada vez que el usuario hace clic, se genera un nuevo Emitter en la posición del mouse, y eliminamos cualquier Emitter anterior para evitar acumulaciones innecesarias. Esto significa que siempre hay un único Emitter en pantalla.
  * Dentro del Emitter, las partículas se crean de dos formas: Cuando se hace clic, se generan 10 partículas iniciales. Mientras el mouse está presionado, el Emitter sigue generando nuevas partículas.

* Desaparición:
  * Cada partícula tiene una vida útil (lifespan) que disminuye con el tiempo. También se eliminan si salen del área del canvas.
  * En cada frame, revisamos si una partícula ha "muerto" y la eliminamos de la lista, lo que ayuda a mantener el rendimiento y la eficiencia en el uso de la memoria.

> 🌳Explica qué concepto aplicaste, cómo lo aplicaste y por qué.

**Concepto aplicado**
*  Apliqué distintas distribuciones:
    *  **Ruido de Perlin para el color** ya que cada partícula tiene valores únicos para R, G y B, que se generan usando Perlin Noise. Estos valores cambian con el tiempo para crear transiciones de color más suaves y orgánicas.
    *  **Distribución normal para la velocidad inicial,**  lugar de asignar velocidades completamente aleatorias, utilicé una distribución normal para que la mayoría de las partículas tengan velocidades cercanas a un valor promedio, con algunas pocas siendo mucho más rápidas o lentas.
    *  **Vuelo de Lévy para el tamaño y movimiento** porque el tamaño de las partículas se genera con una distribución de Vuelo de Lévy, lo que significa que la mayoría son pequeñas, pero ocasionalmente aparecen algunas más grandes. También se aplica a su movimiento para que, en ocasiones, las partículas hagan saltos grandes en direcciones aleatorias.
 

**¿Por qué?**

* El ruido de perlin: genera cambios suaves y naturales, evitando cambios bruscos de color.
* Distribución normal: Hace que la dispersión de las partículas sea más realista pues en el mundo real, la mayoría de los fenómenos físicos siguen distribuciones normales.
* Vuelo de Lévy: Hace que algunas partículas realicen "saltos" grandes en lugar de moverse de manera uniforme generando un efecto más dinámico e impredecible y aumentando la variabilidad visual.

______________________________________________________________________________________________________________________________________

✅ 3. Analiza el ejemplo 4.5: a Particle System with Inheritance and Polymorphism.

✅ 4. Analiza el ejemplo 4.6: a Particle System with Forces.

✅ 5. Analiza el ejemplo 4.7: a Particle System with a Repeller.

PARA CADA UNA DE LAS SIMULACIONES:

> ¿Cómo se está gestionando la creación y la desaparición de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

📤 Entrega:

1. Vas a modificar cada una de las simulaciones anteriores incluyen en cada una, al menos un concepto de las unidades anteriores, pero no repitas concepto, la idea es que repases al menos uno de cada unidad.
2. Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.
3. Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
4. Incluye un enlace a tu código en el editor de p5.js.
5. Incluye el código fuente de cada una de las simulaciones.
6. Captura de pantallas de cada una de las simulaciones con las imágenes que más te gusten como resultado de la ejecución de cada una de las simulaciones.
