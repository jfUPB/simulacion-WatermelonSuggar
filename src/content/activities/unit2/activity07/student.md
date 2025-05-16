### Motion 101

Es un algoritmo que permite simular el movimiento de los objetos en un espacio digital empleando principios físicos básicos como la velocidad, posición y la aceleración. Para ello se ayuda de vectores que representen estas magnitudes físicas, al ser vectores (objetos en p5.js) se pueden manipular con operaciones matemáticas básicas.

**¿Cómo se simula el movimiento?**

* Se crea una clase Mover que contiene atributos de posición, velocidad y aceleración.
* Se actualiza la posición del objeto en cada frame aplicando las ecuaciones anteriores.
* Se usa acceleration para modificar el comportamiento del objeto (por ejemplo, que se mueva hacia el mouse o de manera aleatoria).

**Ejemplo**

Simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/7joZd7T0x): 

* Uno de mis intereses es la pastelería así que quise usar algunos de sus conceptos para poder ilustrarme a mi misma la utilidad de Motion 101. Para realizar tortas suelo usar una batidora de mano, para poder simular el movimiento de este implemento debería prestar atención a su funcionamiento.

📍 Posición 

- La batidora está en un punto fijo dentro del tazón, pero sus aspas giran en círculos.
- Si mueves la batidora dentro del tazón, también cambia su posición.

⚡ Velocidad 

- Cuando enciendes la batidora en velocidad baja, las aspas comienzan a girar lentamente.
- Aumentas la velocidad para mezclar mejor y las aspas giran más rápido.

🚀 Aceleración 

- Si pasas de velocidad 1 a velocidad 5, la batidora no cambia instantáneamente.
- Va aumentando poco a poco hasta alcanzar la velocidad deseada.
- Esto es aceleración, un cambio progresivo en la velocidad.

🌀Inercia 

- Cuando apagas la batidora, no se detiene de inmediato.
- Las aspas siguen girando un poco antes de parar completamente.
- Esto ocurre porque los objetos en movimiento tienden a seguir moviéndose (primera ley de Newton).




