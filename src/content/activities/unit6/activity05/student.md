**Reflexiona sobre los dos algoritmos y tu experiencia:**


### Diferencias fundamentales

* **Flow Fields:** La inteligencia reside principalmente en el entorno. Las partículas simplemente "leen" el campo vectorial y responden a él. No tienen conocimiento de otras partículas. Es como si estuvieran en una corriente invisible.

* **Flocking:** Aquí la inteligencia está en los agentes mismos, quienes siguen reglas simples basadas en la posición y velocidad de sus vecinos (alineación, cohesión y separación). Cada partícula (boid) es consciente de su entorno local inmediato.

* Mi simulación: Reproduce claramente un entorno tipo flow field, pero con toques interesantes como explosiones, glitch zones, y atracción al mouse. Estos elementos enriquecen el campo y, por tanto, el "ambiente" que guía a los agentes.


### Tipos de comportamiento emergente:**

* **Flow Fields:** Facilita trayectorias suaves y fluidas. Es ideal para generar patrones tipo ríos de movimiento, ondas o flujos cerebrales/glitch digitales, como hiciste tú.
Ejemplo: En mi simulación, las partículas parecen crear un patrón coral, dinámico y orgánico cuando el campo es suave, y se vuelve caótico y fragmentado cuando entra el modo glitch o hay explosiones.

* **Flocking:** Produce agrupamientos naturales y patrones como bandadas o cardúmenes. La belleza está en la forma en que pequeñas decisiones locales generan orden colectivo (un grupo que gira o escapa sin haber un “líder”).


### Ventajas y desventajas

* en tu opinión, ¿Cuáles podrían ser las ventajas o desventajas de usar uno u otro algoritmo para ciertos tipos de efectos visuales o simulaciones?

### El agente autónomo

* ¿Cómo te ayudaron estos dos ejemplos (Flow Fields y Flocking) a entender mejor el concepto de “agente autónomo”? ¿Qué características definen a un agente en estos sistemas?


### Emergencia

* ¿En qué momento observaste “comportamiento emergente” (complejidad o patrones no programados explícitamente) al trabajar con estos algoritmos?
