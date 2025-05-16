### Síntesis de lo aprendido

✅ 1. ¿Cómo se gestiona la creación y la desaparición de las partículas y cómo se gestiona la memoria?

* Bueno, las partículas se crean gracias a una clase emitter (emisor) qu es el encargado de generar las partículas y se van almacenando en un arreglo pero para evitar que este colapse u ocupe mucha memoria, se le asigna a cada partícula un tiempo de vida y cuando este se cumple, pum, se elimina del arreglo. 

✅ 2. ¿Cómo se aplica el marco de movimiento motion 101 en los sistemas de partículas que analizaste en la actividad anterior?

* El marco 101 nos permite dotar a nuestros elementos de posición, velocidad y aceleración. Cuando lo aplicamos a las partículas ayuda a que su moviento sea más coherente con las simulaciones que hacemos porque permite que se comporten de manera natural.

✅ 3. ¿Cómo se aplican fuerzas externas a los sistemas de partículas que trabajaste en la unidad? ¿Qué fuerzas se aplicaron y cómo están modeladas?

* Las fuerzas se modelan como vectores, así que se escriben como p5.vector, facilitando que se puedan aplicar operaciones sobre ellas y actúen de manera natural sobre un objeto. En las unidades pasadas pude explorar con la gravedad, el viento y la fuerza de atracción/repulsión. Mi favorita fue la de atracción porque me permitía generar interacciones interesantes entre una partícula y el atractor, sin embargo, la que más usé fue la gravitacional porque es la que más fácil de modelar en los ejemplos que exploramos. 

✅ 4. ¿Cómo se aplicaste los conceptos de herencia y polimorfismo en los sistemas de partículas que trabajaste en la unidad?

* El concepto de herencia me ayudó a crear partículas de diferentes formas que compartieran un molde en común. En el caso de la entrega final de esta unidad, el molde original era la clase particle.js y la hija que heredaba estas características era confetti.js. Por su parte, el polimorfismo me ayudó en la implementación de comportamientos similares sobre diferentes clases porque en mi programa tenía varias clases de partículas en cuanto a forma y color pero que a su vez cumplían muchas veces con el mismo movimiento.
