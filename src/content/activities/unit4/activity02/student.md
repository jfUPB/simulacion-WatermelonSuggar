**Simulación 1**

> ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?

  En este código, se está creando una animación en la que una línea y dos círculos rotan alrededor de su punto central. 
La interacción se da cuando presionas una tecla, ya que al presionar cualquier tecla, la variable angle aumenta en 0.1, lo que hace que los elementos gráficos rotan.
  
> Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?

  La instrucción translate(width / 2, height / 2); mueve el origen del sistema de coordenadas al centro de la pantalla. Esto es útil porque facilita la manipulación y rotación de objetos alrededor del centro de la ventana, sin tener que calcular desplazamientos manualmente. 
  Cuando el origen está en el centro, todo lo que se dibuje (líneas, círculos, etc.) se rotará alrededor de este centro.

> Cuál es la relación entre el sistema de coordenadas y la función rotate().

  La función rotate() rota los objetos alrededor del origen del sistema de coordenadas. Al mover el origen con translate(), la rotación afectará a los objetos en relación con el nuevo centro (que ahora está en el centro de la pantalla).
  Sin translate(), la rotación se aplicaría respecto al origen por defecto (en la esquina superior izquierda).
