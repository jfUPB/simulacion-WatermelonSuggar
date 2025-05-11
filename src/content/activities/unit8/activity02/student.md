### Conceptualización

**Definición del concepto**

* Me gustaría explorar la metáfora de lo oculto, la búsqueda de respuestas más allá de lo que que se observa a simple vista, Una exploración visual inspirada en "Los dioses ocultos" de Caifanes. La obra evoca una atmósfera entre _lo místico y lo caótico_, donde lo orgánico y lo geométrico se entrelazan para sugerir una búsqueda interior. Lo que no se ve, se manifiesta a través del sonido.

**Pieza musical elegida**

["Los dioses ocultos" – Caifanes](https://www.youtube.com/watch?v=5jMFlNiuszQ)

**Inputs de audio**

1. Amplitud (Volumen general)

* Descripción técnica: Valor entre 0 y 1 que indica el volumen promedio en un instante.

> Uso conceptual:
> * A mayor volumen, más elementos visuales emergen desde el fondo o capas ocultas. Representa el momento en que “los dioses ocultos” se manifiestan.
> * El tamaño de los elementos orgánicos o geométricos crece o pulsa con el volumen, generando una sensación de latido vital o invocación.
> * El número de partículas, líneas o nodos aumenta con la amplitud, reforzando la sensación de caos o complejidad.

2. FFT (Transformada Rápida de Fourier – análisis de espectro de frecuencias)

* Se extraen energías de distintas bandas para controlar distintos aspectos visuales.

* Banda de Graves (20–250 Hz)
Uso técnico: fft.getEnergy("bass")

> Uso conceptual:
> * Controla la base estructural de la escena: formas pesadas, raíces o elementos que se expanden desde el centro o el fondo.
> * Representa lo ancestral, lo enterrado o lo profundo.

* Banda de Medios (250–2000 Hz)
Uso técnico: fft.getEnergy("mid")

> Uso conceptual:
> * Controla las distorsiones geométricas: ondulaciones, quiebres o cambios de forma en estructuras previamente regulares.
> * Genera la tensión visual entre orden y caos.

* Banda de Agudos (2000–8000 Hz)
Uso técnico: fft.getEnergy("treble")

> Uso conceptual:
> * Controla la velocidad de partículas o aparición de destellos y líneas finas.
> * Representa lo etéreo, lo invisible que se deja ver brevemente, como un destello de comprensión o una revelación fugaz.

**Inputs de interacción**

1. Mouse (posición y clic)
  
* Posición X/Y:
  * X: Transición entre tipos de geometría (circular ↔ triangular ↔ fractal), revelando diferentes facetas del “oculto”.
  * Y: Control del nivel de deformación caótica en las figuras, como si el usuario “rasgara el velo”.

* Click:
  * Activa una capa oculta visual: símbolos, texto o patrones que aparecen brevemente como si fueran mensajes revelados al tacto.

* Teclado
  * Cambia la paleta de colores, alterna entre tonos sombríos (oscuros, misteriosos) y saturados (revelación, energía).
 

**RESUMEN DE INPUTS**


| Tipo        | Variable Técnica                     | Uso Visual / Conceptual                            |
| ----------- | ------------------------------------ | -------------------------------------------------- |
| Amplitud    | `amplitude.getLevel()`               | Escala, densidad, intensidad visual (revelación)   |
| FFT Graves  | `fft.getEnergy("bass")`              | Estructuras profundas, raíces, lo oculto ancestral |
| FFT Medios  | `fft.getEnergy("mid")`               | Distorsión, caos geométrico                        |
| FFT Agudos  | `fft.getEnergy("treble")`            | Partículas, destellos, revelaciones fugaces        |
| Mouse X     | `mouseX`                             | Cambia geometrías base                             |
| Mouse Y     | `mouseY`                             | Ajusta caos/deformación                            |
| Mouse Click | `mouseIsPressed`                     | Revela capas ocultas (símbolos/texto)              |
| Teclas      | `keyPressed()`                       | Cambia paleta de colores                           |
