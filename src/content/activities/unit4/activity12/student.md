analiza tu obra de arte generativa respondiendo las siguientes preguntas:

> **¿Qué concepto de oscilación utilizaste como base para tu obra? Describe cómo lo implementaste.**

Me basé en la oscilación sinusoidal para definir el movimiento de las partículas. La idea era que no solo se movieran hacia el mouse, sino que lo hicieran con trayectorias ondulantes, agregando más fluidez y variación en los patrones.

**Implementación:**

* Cada partícula tiene su propia amplitud, frecuencia y fase aleatoria, lo que hace que las oscilaciones sean distintas entre ellas.
* En lugar de un movimiento recto, las partículas siguen caminos que combinan atracción y oscilación, con un factor que decide si se moverán más en zigzag o de manera más directa.
* Usé sin(frameCount * frecuencia + fase) para modificar sus trayectorias, haciendo que el movimiento no sea repetitivo ni predecible.

> **¿Cómo funciona la interacción en tu obra? Explica cómo el usuario puede modificar la obra en tiempo real.**

La interacción se da con el movimiento del cursor. El usuario no controla las partículas directamente, pero sí influye en su trayectoria.

**Lo que pasa en tiempo real:**

* El cursor funciona como atractor y algunas partículas lo siguen de forma más directa, otras oscilan alrededor de su posición.
* No todas las partículas reaccionan igual, algunas se ven más afectadas por la oscilación que otras.
* Si el usuario mueve el mouse rápido, se pueden generar estelas más caóticas, mientras que movimientos lentos producen ondulaciones más suaves.


> **¿Qué desafíos encontraste durante el proceso de creación? ¿Cómo los superaste?**

* Hice muchas versiones, las primeras eran bastante aburridas y no tenían ningún tipo de interacción, sin embargo, seguí implementando funciones y conceptos. La versión tres fue mi favorita pero no aplicaba ninguno de los conceptos vistos en la unidad, igualmente me fue de mucha utilidad para replantear la propuesta y que siguiera teniendo el mismo concepto.
* Hubo versiones en las que las oscilaciones eran iguales y se sentía artificial. Para romper la monotonía, agregué un desfase aleatorio (phaseOffset) en cada partícula, así no se mueven al mismo ritmo.
* La atracción al mouse a veces cancelaba el movimiento sinusoidal por lo que las partículas terminaban siguiendo trayectorias rectas así que ajusté la mezcla entre la fuerza de atracción y la oscilación para que ambas coexistieran sin que una dominara completamente.
* Como son muchas partículas el sketch se volvía lento en ciertos momentos y tuve que optimizar el código, reduciendo la cantidad de cálculos por partícula y evitando dibujar demasiadas líneas en cada frame.

> **¿Qué aprendiste sobre las oscilaciones y su aplicación en el arte generativo?**

* Las oscilaciones bien implementadas pueden hacer que un sistema parezca más orgánico y dinámico, en lugar de moverse de forma mecánica.
* Pequeñas variaciones en la frecuencia y fase cambian completamente la percepción del movimiento, haciendo que la obra tenga más vida.
* El equilibrio entre control y caos es clave: si todo se mueve igual, es aburrido; si todo es demasiado aleatorio, pierde coherencia.
* Experimentar con muchas ideas abre posibilidades para generar patrones y texturas visuales interesantes sin necesidad de gráficos ni cálculos complejos.

