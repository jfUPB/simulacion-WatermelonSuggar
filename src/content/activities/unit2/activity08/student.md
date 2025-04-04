
**Modificaciones propuestas**

1. Que pasaría si cada vez que el objeto desaparezca pueda tener una ligera variación de color (utilizando el random gaussian para mantener una paleta de color análoga a los lilas)

2. Que pasaría si en cada paso de la bola por el canvas se va dibujando el camino por el que pasa justo con el color que tenga en ese momento, que al reaparecer la bola de otro color se mantenga el rastro de la anterior y así sucesivamente para poder crear una obra de arte generativo 

**¿Qué te imaginas que pasará?**

* Espero que se pueda crear una obra abstracta interactiva con los colores que tenga la bola en cada generación.

**¿Qué pasó?**

* **Primera versión**

![image](https://github.com/user-attachments/assets/7cbac11e-ad7d-4ded-af93-5ae530a19e41)

Sí se estaban generando los rastros del paso de cada círculo generadop pero de una manera lenta y monótona, así que decidí modificarlo un poco porque el código no estaba variando la velocidad y la variación de la velocidad era corta.

* **Segunda versión**

Simulación [AQUÍ](https://editor.p5js.org/WatermelonSuggar/sketches/Qvd5FZLLE)

![image](https://github.com/user-attachments/assets/0b89195e-1567-461f-904b-602626b7835e)

A esta segunda versión le implementé los siguientes cambios:

* Aumenté el rango de la velocidad inicial de un random(-2,2) a un random(-5,5) para que el movimiento sea más rápido.
* Para la aceleración empleé el ruido de Perlin para que las variaciones sean más fluidos y naturales.
* Limité la velocidad para evitar movimientos extremadamente caóticos.
* Hay un pequeño cambio dinámico en el tamaño del círculo

**¿Por qué?**

* En la primera versión estaba implementando el concepto pero de manera muy "tiesa", no le estaba dando tanta vida a las generaciones por lo que eran aburridas y monótonas. Para la segunda versión quise ver cómo podría implementar los conceptos de manera divertida y que parecielas "Hilos que tejen" para darle un poco de vida al ejemplo.

**Concluye**

* Darle vida al concepto hizo que pensara en ideas divertidas para tener proyectos personales, quisiera implementar el concepto en el desarrollo de una idea relacionada con el crochet que quiero seguir explorando. 
