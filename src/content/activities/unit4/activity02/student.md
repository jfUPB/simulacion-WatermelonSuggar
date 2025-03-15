**Simulación 1** 

[Manejo de ángulos](https://torrentio.strem.fun/manifest.json)

> ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?

  En este código, se está creando una animación en la que una línea y dos círculos rotan alrededor de su punto central. 
La interacción se da cuando presionas una tecla, ya que al presionar cualquier tecla, la variable angle aumenta en 0.1, lo que hace que los elementos gráficos rotan.
  
> Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?

  La instrucción **translate(width / 2, height / 2)**; mueve el origen del sistema de coordenadas al centro de la pantalla. Es decir, quepara realizar el desplazamiento no se hace manualmente sino que se realiza una rotación en el centro del canvas. Cuando el origen está en el centro, todo lo que se dibuje (líneas, círculos, etc.) se rotará alrededor de este centro.

> Cuál es la relación entre el sistema de coordenadas y la función rotate().

  La función **rotate()** rota los objetos alrededor del origen del sistema de coordenadas. Al mover el origen con **translate()**, la rotación afectará a los objetos en relación con el nuevo centro (que ahora está en el centro de la pantalla). Sin translate(), la rotación se aplicaría respecto al origen por defecto (en la esquina superior izquierda).

> Observa que al dibujar los elementos gráficos parece que se está dibujando en la posición (0, 0) del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

Los elementos se dibujan con coordenadas relativas al origen (0,0), pero como translate(width / 2, height / 2) mueve el origen al centro de la pantalla, todo se posiciona alrededor de ese punto. Aunque el código de dibujo es el mismo en cada frame, rotate(angle) aplica una rotación acumulativa. Como angle aumenta con cada tecla presionada, los objetos giran progresivamente en torno al centro.

