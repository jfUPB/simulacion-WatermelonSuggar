**Reflexiona sobre los dos algoritmos y tu experiencia:**

### Diferencias fundamentales

* **Flow Fields:** La inteligencia reside principalmente en el entorno. Las partículas simplemente "leen" el campo vectorial y responden a él. No tienen conocimiento de otras partículas. Es como si estuvieran en una corriente invisible.

* **Flocking:** Aquí la inteligencia está en los agentes mismos, quienes siguen reglas simples basadas en la posición y velocidad de sus vecinos (alineación, cohesión y separación). Cada partícula (boid) es consciente de su entorno local inmediato.

* Mi simulación: Reproduce claramente un entorno tipo flow field, pero con toques interesantes como explosiones, glitch zones, y atracción al mouse. Estos elementos enriquecen el campo y, por tanto, el "ambiente" que guía a los agentes.


### Tipos de comportamiento emergente

* **Flow Fields:** Facilita trayectorias suaves y fluidas. Es ideal para generar patrones tipo ríos de movimiento, ondas o flujos cerebrales/glitch digitales, como hiciste tú.
Ejemplo: En mi simulación, las partículas parecen crear un patrón coral, dinámico y orgánico cuando el campo es suave, y se vuelve caótico y fragmentado cuando entra el modo glitch o hay explosiones.

* **Flocking:** Produce agrupamientos naturales y patrones como bandadas o cardúmenes. La belleza está en la forma en que pequeñas decisiones locales generan orden colectivo (un grupo que gira o escapa sin haber un “líder”).


### Ventajas y desventajas

> En tu opinión, ¿Cuáles podrían ser las ventajas o desventajas de usar uno u otro algoritmo para ciertos tipos de efectos visuales o simulaciones?

* **FLOW FIELDS**

  * VENTAJA:
    
    * Se pueden lograr efectos visuales complejos sin la necesidad de hacer muchos cálculos
      
  * DESVENTAJA:
    
    * Como no hay interacción entre partículas, puede ser un poco aburrido o parecer artificial si no se enriquece su concepto.

* **FLOCKING**

  * VENTAJA:
    
    * Es una simulación más relacionada con fenómenos naturales y colectivos, así que es ideal para simular vida y comoportamientos biológicos.
      
  * DESVENTAJA:
    
    * Como las partículas analizan el movimiento de sus compañeritas se hacen más cálculos computacionales y si se quieren modificar comportamientos es más dificíl de controlar el comportamiento de las partículas.

### El agente autónomo

> ¿Cómo te ayudaron estos dos ejemplos (Flow Fields y Flocking) a entender mejor el concepto de “agente autónomo”? ¿Qué características definen a un agente en estos sistemas?

* Bueno en ambos algoritmos identifiqué las siguientes características del agente autónomo: tiene una posición, velocidad, aceleración, decide cómo moverse en función de inputs locales (campo o vecinos) y actúa de manera independiente, pero con reglas comunes.

En el caso de mi código experimental, cada partícula decide qué hacer dependiendo del campo y de su cercanía al mouse o a una explosión. 

### Emergencia

> ¿En qué momento observaste “comportamiento emergente” (complejidad o patrones no programados explícitamente) al trabajar con estos algoritmos?

* Bueno, cuando se activa el modo glitch pasa algo en particular y es que el sistema se vuelve un poco caótico pero coherente porque las partículas no se enlocan del todo, solo cambian de comportamiento que aunque sea aleatorio parece que tuviera una lógica programada detrás.

* Las partículas rodean el mouse y parece que intentan "trabajar juntas" y no está programado en el patrón sino que las fuerzas que estpy usando hacen que eso suceda.

* Las explosiones son la parte más dovertida porque dispersa naturalmente a las partículas que huyen como si quisieran salvarse.
  
