> ¿Cuál es la relación entre r y theta con las posiciones x y y?

* **r (radio)** es la distancia desde el origen.
* **θ (ángulo)** es la dirección en la que se encuentra el punto con respecto al eje 𝑥

Ambos se combinan para definir la ubicación de un punto en el plano de forma radial y angular. 
Este sistema es particularmente útil cuando trabajamos con fenómenos que tienen simetría radial o circular, como la trayectoria de planetas, ondas, o incluso en gráficos y simulaciones.

**Modificaciones**

> Modificación 1

* **Problema:** en consola sale un **ReferenceError: x is not defined**. Utiliza un vector unitario (de magnitud 1) sin considerar el valor de r, por lo que la posición del círculo y la longitud de la línea no cambian en función de r. El movimiento será constante, con una distancia fija de 1 unidad desde el origen.

* **Solución:** reemplazar x e y por las componentes del vector v, que son v.x y v.y. Como v es un vector creado con fromAngle(), estas propiedades representan las coordenadas x y y del vector unitario.

> Modificación 2

* El vector creado ahora tiene una longitud igual a r. El círculo y la línea se dibujarán correctamente en función de la distancia del origen definida por r y la dirección dada por el ángulo theta. Además, la posición del círculo se desplazará de acuerdo con theta, haciendo que se mueva en un círculo alrededor del centro de la pantalla.
  
