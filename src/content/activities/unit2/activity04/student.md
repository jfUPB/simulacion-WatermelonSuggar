* ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). 
  ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?

    **Método mag():** Devuelve la magnitud (o norma) del vector, es decir, su longitud en el espacio calculando la raíz cuadrada de las dimensiones del vector.
    
    ![image](https://github.com/user-attachments/assets/13386217-4c77-4bf6-a306-c0a911765ad9)

    **Método magSq():** Devuelve la magnitud al cuadrado del vector, sin calcular la raíz cuadrada.

    ![image](https://github.com/user-attachments/assets/66cd3155-db27-4bbe-b8e8-a5cd85a09ff2)

    **Diferencias**
    * mag() calcula la raíz cuadrada, lo cual consume más recursos computacionales.
    * magSq() es más eficiente si solo necesitas comparar magnitudes (evitando la raíz cuadrada innecesaria).

    ⭐El método más eficiente es **magSq()** porque consume menos recursos al no calcular la raíz cuadrada

* ¿Para qué sirve el método normalize()?
* Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
* El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?
* Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
* ¿Para que te puede servir el método dist()?
* ¿Para qué sirven los métodos normalize() y limit()?
