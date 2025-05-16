**¿Cómo modelar una fuerza para una simulación?**

**1. Entendder el concepto detrás de una fuerza:** Una fuerza es una interacción que puede cambiar el estado de movimiento de un objeto. Las fuerzas pueden ser resultado de distintas interacciones como la gravedad, la fricción, el empuje, etc.
  
**2. Descomponer la fórmula de la fuerza en dos partes:**
   * Dirección de la fuerza: La dirección de una fuerza indica hacia dónde se aplica. Por ejemplo, la gravedad siempre actúa hacia abajo.
   * Magnitud de la fuerza: La magnitud de una fuerza indica cuánta fuerza se aplica. Puede calcularse usando fórmulas específicas para cada tipo de fuerza.

**3. Representar la fuerza como un vector:** En programación, una fuerza se representa como un vector, que tiene una dirección y una magnitud. En "Nature of Code", se utilizan las clases PVector o Vector para representar fuerzas.
  
**4. Crear una clase que represente el objeto en movimiento:** En esta clase, define propiedades como posición, velocidad y aceleración, que también son vectores. Además, deberás incluir una propiedad de masa, que afectará cómo se aplica la fuerza al objeto.

**5. Aplicar la fuerza al objeto:** Para aplicar una fuerza a un objeto, utiliza la segunda ley de Newton: 𝐹=𝑚*𝑎. Esto significa que la fuerza es igual a la masa del objeto multiplicada por su aceleración. Para obtener la aceleración, divide la fuerza por la masa del objeto.

**6. Actualizar el estado del objeto:** En cada frame de la simulación, actualiza la posición y la velocidad del objeto sumando la aceleración a la velocidad y la velocidad a la posición.
   
**7. Implementar la función de dibujo:** Crea una función que dibuje el objeto en la pantalla, utilizando su posición actualizada.

**8. Añadir las fuerzas adicionales:** Si quieres simular otras fuerzas, como la fricción o el arrastre, simplemente crea funciones adicionales que calculen esas fuerzas y aplícalas al objeto de la misma manera que aplicaste la fuerza inicial.
