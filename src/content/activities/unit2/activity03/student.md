* ¿Qué resultado esperas obtener?
  * Si le agrego **console.log(posicion.toString());** en diferentes partes del código, puedo ver el comportamiento de la posición en algunas partes del código.
    
* ¿Qué resultado obtuviste?

  ![image](https://github.com/user-attachments/assets/922134f1-25c9-4abe-9305-683de82ce7b3)
  
* Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.

  * En **paso por valor**, la función recibe una copia de la variable original, por lo que cualquier cambio no afecta el valor original.
    * **_Tipos de datos a los que aplica:_** Number, String, Boolean, Undefined, Null, Symbol...

    ```js
    let a = 10;
    let b = a;  // Se copia el valor
    
    b = 20; // Solo cambia 'b', 'a' sigue siendo 10
    
    console.log(a); // 10
    console.log(b); // 20
    ```

  * En paso por referencia, la función recibe una referencia (una dirección en memoria) al objeto original, lo que significa que cualquier cambio
    dentro de la función afectará la variable original.
    * **_Tipos de datos a los que aplica:_** Object (p5.Vector entra en esta categoría), Array, Function

    ```js
    let miVector = createVector(10, 20);
    console.log("Antes:", miVector.toString()); // (10, 20)

    function modificarVector(v) {
    v.x = 50;
    v.y = 75;
    }
    
    modificarVector(miVector);
    console.log("Después:", miVector.toString()); // (50, 75)
    ```

* ¿Qué tipo de paso se está realizando en el código?
  * Los cambios en la posición del vector se dan debido a que es un objeto, y **los objetos en JavaScript se pasan por referencia**, lo que significa que cualquier modificación dentro de playingVector(v)
  afecta directamente la variable posicion.

* ¿Qué aprendiste?
  * **p5.Vector** se comporta como un objeto en JavaScript, por lo que cualquier modificación dentro de una función afecta la variable original.
  * Usar **console.log()** con **.toString()** es útil para depurar y entender cómo cambian los valores en el código.
  * Los números y strings en JavaScript se pasan por valor (se copia el dato).
  * Los objetos, arreglos y vectores de p5.js se pasan por referencia, lo que significa que modificar un objeto dentro de una función modifica el original.
