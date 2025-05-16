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
  * Ajusta el vector para que tenga una magnitud de 1, sin cambiar su dirección.
  * La versión estática de la función -> p5.vector.normalize(v), devuelve un nuevo vector sin modificar el original.
 
* Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
  
  * El **método dot()** es como una entrevista: mide qué tanto coincide la dirección de tus preguntas (un vector) con las respuestas del entrevistado (otro vector). Si el resultado es alto, están alineados; si es cero, no hay relación; y si es negativo, van en direcciones opuestas.

* El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?

|Versión| Sintaxis | ¿Cómo funciona? | ¿Cuándo usala?|
|-------|----------|-----------------|---------------|
| Instancia| vectorA.dot(vectorB) | Se llama desde un vector específico | Cuando ya tienes un vector base y quieres compararlo con otro|
|Estática| p5.Vector.dot(vectorA, vectorB) | Se llama desde **p5.Vector** y usa dos vectores dados | Cuando quieres calcular el producto escalar sin depender de un vector en particular|
  
* Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.

  * El producto cruz entre dos vectores es una operación que produce un nuevo vector perpendicular al plano definido por los dos vectores originales.
 
  * Geometría del producto cruz
  *   **Magnitud:** La magnitud del vector resultante es igual al área del paralelogramo formado por los dos vectores originales.
    *   Si los vectores son paralelos, el área es cero y el producto cruz también.
    *   Si son perpendiculares, el área es máxima.
    
  *   **Dirección (Orientación):**
    *   La dirección del vector resultante sigue la regla de la mano derecha:
  
  ![image](https://github.com/user-attachments/assets/3a26d895-512a-46a2-9a21-92874a154163)
  
    * El pulgar apuntará en la dirección del vector resultante.
    * En 3D, el vector resultante es perpendicular al plano definido por los dos vectores originales.
    * En 2D, el producto cruz da un escalar (no un vector), que representa cuánto "gira" un vector respecto al otro.

* ¿Para que te puede servir el método dist()?
  
  * El **método dist()** calcula la distancia entre dos puntos en un espacio 2D o 3D. Es útil para: Detectar colisiones entre objetos, medir la proximidad entre dos elementos y aplicar reglas basadas en distancia (por ejemplo, hacer que un personaje se mueva solo si está cerca de un objetivo).
    
* ¿Para qué sirven los métodos normalize() y limit()?
  *  **normalize()** - Convertir un vector en unitario
    * El método .normalize() transforma un vector en un vector unitario, es decir, un vector con la misma dirección, pero con magnitud 1.

  ¿Por qué es útil?
  * Se usa cuando solo te interesa la dirección y no la magnitud.
  * Permite manejar direcciones sin alterar la escala de otros cálculos.
________________________________________________________________________________________________________
 
  * **limit()** - Restringir la magnitud de un vector
  
    * El método .limit(max) asegura que la magnitud de un vector no supere un valor dado.

¿Para qué sirve?
* Evita que un objeto se mueva demasiado rápido en una simulación.
* Controla la velocidad de partículas o agentes en inteligencia artificial.
