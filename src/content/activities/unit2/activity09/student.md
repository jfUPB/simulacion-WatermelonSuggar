**Simulación del experimento**

Link [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/jOVYeM_Yd)

![image](https://github.com/user-attachments/assets/81dd5a32-99e5-4be7-bd3c-8df1726a6e4e)

**Explicación del experimento**

Para realizar el experimento pensé en crear una simulación que contuviera tres circulos y que cada uno de ellos tuviese un tipo de aceleración de acuerdo a las instrucciones (Aceleración constante, Aceleración aleatoria y Aceleración hacia el mouse). Su aparición en pantalla es simultánea para poder realizar las comparaciones, el canvas se divide en tres franjas verticales y en cada franja estará una bolita que al tocar la franja límite que le corresponde debe rebotar para mantenerse dentro de dicho limite.

**Observaciones**

* **Aceleración constante**: La bolita comienza con un pequeño impulso y va ganando velocidad de manera progresiva en una dirección fija. Rebota en los bordes, pero su movimiento es predecible y suave. En este caso, la aceleración es una constante fija en el eje X, lo que hace que la velocidad aumente de forma continua. Como la aceleración no cambia de dirección, el movimiento es uniforme y estable.

* **Aceleración aleatoria:** La bolita se mueve de manera errática, cambiando constantemente de dirección y velocidad. No sigue un patrón definido y puede parecer caótica. Cada ciclo de dibujo, la aceleración se elige aleatoriamente, lo que afecta la velocidad y dirección del movimiento. Esto simula un comportamiento más desordenado, similar al movimiento de partículas en un líquido o gas.

* **Aceleración hacia el mouse:** La bolita se mueve de manera fluida y se dirige hacia el cursor del mouse. Si el mouse se mueve rápidamente, la bolita tarda en alcanzarlo debido a la inercia. Aquí, la aceleración está determinada por la diferencia entre la posición de la bolita y la del mouse. Al hacer que esta diferencia sea un vector de aceleración, la velocidad se ajusta para que la bolita siempre intente alcanzar el mouse, simulando un efecto de atracción.
