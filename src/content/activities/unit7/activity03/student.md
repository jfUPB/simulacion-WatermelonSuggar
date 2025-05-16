### Tipografía animada

Palabra elegida: **COLAPSAR**

**Representación:**
* La palabra "COLAPSAR" será representada mediante un conjunto de bloques dispuestos en una matriz que forma cada letra. Cada letra de la palabra está compuesta por varios bloques que se organizan según una cuadrícula (matriz) definida. Cada bloque es un cuerpo físico en Matter.js, lo que permite simular el comportamiento físico de la palabra cuando se colapsa.

**Comportamiento físico**
* El comportamiento representado por el significado de "COLAPSAR" será la caída de los bloques de cada letra de manera desordenada. Al hacer clic, los bloques se convierten en dinámicos y caen, dispersándose y rebotando ligeramente gracias a la restitución (elasticidad), simulando un colapso o desmoronamiento.

**Configuración de Matter.js**

* La gravedad se define en Matter.js por defecto, lo que asegura que los bloques caigan hacia el suelo. Puedes ajustar la gravedad si necesitas un comportamiento diferente.

* El límite del mundo se define mediante el cuerpo estático ground, que actúa como el "suelo" sobre el cual los bloques caen y rebotan.

* Los bloques inicialmente son estáticos y se convierten en dinámicos cuando se hace clic.
  
  * Cuando se hacen dinámicos, tienen:
    * La masa por defecto es determinada por Matter.js, pero se puede ajustar.
    * La fricción se ajusta a 0.3 en los bloques para que no se deslicen demasiado rápido.
    * La restitución de la elasticida se establece en 0.4 para que los bloques reboten ligeramente cuando caen.
      
**Interacción**
* Cuando el usuario hace clic en el lienzo (mousePressed), los bloques de las letras se convierten de estáticos a dinámicos, lo que provoca el colapso de la palabra "COLAPSAR". Al hacer clic, se aplica una pequeña fuerza hacia abajo (y ligeramente lateral) sobre cada bloque para simular el colapso desordenado.


**Experimento**

Enlace [aquí](https://editor.p5js.org/WatermelonSuggar/full/bXOsN1jV-)

![image](https://github.com/user-attachments/assets/192ceb3b-717b-4d57-a842-a270f505d6ed)

![image](https://github.com/user-attachments/assets/89f90670-31bc-4f48-9e08-038e6c350a11)

