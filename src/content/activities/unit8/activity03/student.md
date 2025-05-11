### Diseño de algoritmo generativo

1. ¿Cuál será la lógica central que genere los visuales?

Mi sistema se centrará en un campo de flujo dinámico que evoluciona en respuesta al FFT y que guía a una nube de partículas (elementos visuales orgánico-geométricos). Estas partículas no solo reaccionan al audio, sino que “buscan” lo oculto (zonas de mayor energía) y se transforman visualmente con la interacción del usuario. 
La visualización se compone de múltiples capas dinámicas que emergen y evolucionan en respuesta al análisis de la música “Los dioses ocultos” de Caifanes. El corazón del sistema está compuesto por:

* Un sistema de partículas que se mueve guiado por un campo vectorial (flow field).
* Una red de conexiones temporales entre partículas que se activan con picos de amplitud.
* Una capa oculta de símbolos visuales que se revelan solo en presencia de alto volumen, haciendo alusión al concepto de verdades invisibles que emergen con la música.

> Este sistema combina:
> * Generación de partículas con fuerzas dinámicas.
> * Campos vectoriales dinámicos modulados por el análisis de frecuencia (FFT).
> * Uso de Perlin noise, aleatoriedad controlada, y condiciones iniciales variables.
> * Elementos interactivos que modifican parámetros visuales.

2. Outputs visuales

* Partículas orgánicas: puntos deformables que dejan estelas o se conectan entre sí.
* Geometrías pulsantes: figuras semirregulares que vibran o se distorsionan con Perlin noise.
* Líneas de conexión entre partículas cercanas (solo en ciertas bandas de frecuencia).
* Símbolos ocultos (dibujos, palabras, signos) que emergen en picos o al clic.

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
| `mouseIsPressed`          | Revela símbolos/textos ocultos brevemente.                                     |
| `keyPressed()`            | Cambia paletas visuales o sensibilidad del sistema.                            |


**Referencia visual**

![image](https://github.com/user-attachments/assets/98e2930c-9199-437d-b2e2-e252b8f3e677)
