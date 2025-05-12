### Diseño de algoritmo generativo

1. ¿Cuál será la lógica central que genere los visuales?

El sistema se fundamenta en un campo de flujo dinámico que responde a la estructura y energía del audio, específicamente al análisis FFT de The Emptiness Machine. Este flujo guía una nube de partículas compuesta por elementos orgánico-geométricos que representan el caos y la introspección presentes en la canción.

La visualización emerge en capas dinámicas que reflejan el contraste entre lo estructurado y lo desordenado, evocando la dualidad de vacío y búsqueda que inspira la obra.

* Un sistema de partículas que se mueve guiado por un campo vectorial (flow field).
* Una red de conexiones temporales entre partículas que se activan con picos de amplitud.
* Una capa oculta de símbolos visuales que se revelan al hacer click en la pantalla.
  
> Este sistema combina:
> * Generación de partículas con fuerzas dinámicas.
> * Campos vectoriales dinámicos modulados por el análisis de frecuencia (FFT).
> * Uso de Perlin noise, aleatoriedad controlada, y condiciones iniciales variables.
> * Elementos interactivos que modifican parámetros visuales.

2. Outputs visuales

* Partículas orgánicas: puntos deformables que dejan estelas o se conectan entre sí.
* Geometrías pulsantes: figuras semirregulares que vibran o se distorsionan con Perlin noise.
* Símbolos ocultos que emergen en picos o al clic.

3. Propiedades controladas dinámicamente:

* Posición y velocidad: moduladas por el flow field y energía en medios/agudos.
* Tamaño: según la amplitud.
* Forma: distorsionada por Perlin modulado por mouseX, fft.

4. Técnicas y conceptos de simulación usados

 * Sistema de partículas (capas múltiples).
 * Flow Field basado en ruido Perlin, modulado por energía en graves.
 * Uso de FFT para modulación de comportamiento según bandas de frecuencia.
 * Revelación progresiva de símbolos ocultos por medio de la amplitud.
 * Aleatoriedad controlada para asegurar diversidad en cada ejecución.
 * Interacción directa con el usuario a través del mouseX y teclado.

5. Mapeo de Inputs: parámetros generativos

| **Input**                 | **Afecta a**                                                                   |
| ------------------------- | ------------------------------------------------------------------------------ |
| `amplitude.getLevel()`    | Cantidad de partículas emitidas + tamaño de formas + opacidad de revelaciones. |
| `fft.getEnergy("bass")`   | Rotación del flow field, fuerza de arrastre, distorsión geométrica.            |
| `fft.getEnergy("mid")`    | Velocidad y dispersión de partículas, color base.                              |
| `fft.getEnergy("treble")` | Aparición de destellos, estallidos o conexiones entre partículas.              |
| `mouseX` / `mouseY`       | Factor de deformación geométrica (X), nivel de caos (Y).                       |
| `mouseIsPressed`          | Revela símbolos ocultos brevemente.                                            |
| `keyPressed()`            | Cambia paletas visuales o sensibilidad del sistema.                            |

