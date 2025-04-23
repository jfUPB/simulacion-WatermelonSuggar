## Análisis de los enjambres (Flocking)

### Las 3 reglas

**1. Separación:** evita colisiones o que los boids estén demasiado juntos. Lo hace de la siguiente manera: 

1. Revisa los demás boids a su alrededor (dentro de un cierto radio).
2. Si encuentra a otros boids muy cerca (más cerca que desiredSeparation), calcula un vector en dirección contraria para alejarse.
3. Si hay varios boids cerca, hace un promedio de esas direcciones.

La variable clave es:

```js
let desiredSeparation = 25; // qué tan cerca es “demasiado cerca”
```

**2. Alineación:** los vehículos se mueven en la misma dirección promedio que sus vecinos.

1. Encuentra los boids cercanos dentro de un radio (neighborDistance).
2. Promedia las velocidades (dirección + rapidez) de sus vecinos cercanos.
3. Intenta ajustar su velocidad para que coincida con ese promedio.

La variable clave es: 

```js
let neighborDistance = 50; // distancia para considerar vecinos
```


**3. Cohesión:** evita que el grupo se disperse haciednod que se acerquen al centro del grupo cercano.

1. Identifica a los vecinos cercanos dentro de un radio (neighborDistance).
2. Calcula el centro de masa (posición promedio) de los boids cercanos.
3. Calcula un vector hacia ese punto central.
4. Usa seek(target) para calcular una fuerza que lo lleve hacia ese centro.

La variable clave es: 

```js
let neighborDistance = 50; // distancia para considerar vecinos
```

> Igual que la alineación.
