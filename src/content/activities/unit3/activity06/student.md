**¿Por qué es necesario multiplicar la aceleración por cero al final de cada frame?**

El orden de los cálculos que se realizan en el método update() es: primero se usan todas las fuerzas aplicadas para actualizar la velocidad y la posición; luego, se borra la aceleración **(this.acceleration.mult(0))** para evitar que se acumule en el siguiente frame.
Finalmente, se vuelven a aplicar las fuerzas y se recalcula la aceleración desde cero. 

Así se asegura que el objeto responde correctamente a las fuerzas en cada frame sin errores de acumulación no deseada.
