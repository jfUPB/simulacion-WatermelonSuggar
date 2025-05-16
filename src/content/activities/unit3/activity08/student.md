**¿Cuál es la diferencia entre las dos líneas?**

```js
let friction = this.velocity.copy();
```

* En esta **primera línea** se está pasando **por valor**, ya que copy() crea una nueva instancia del vector con los mismos valores que this.velocity, pero sin compartir la referencia.
* friction es ahora un objeto independiente de this.velocity, por lo que modificar friction no afectará this.velocity.
* this.velocity.copy(); → Crea un nuevo objeto independiente (valor).

```js
let friction = this.velocity;
```

* En la **segunda línea**, sí se usa el **valor por referencia**, porque friction y this.velocity apuntan al mismo objeto en memoria. Cualquier cambio en friction afectará también a this.velocity.
* this.velocity; → Apunta al mismo objeto (referencia).

**¿Qué podría salir mal con let friction = this.velocity;**

Estaríamos modificando this.velocity directamente en lugar de solo modificar friction. Esto puede generar comportamientos inesperados en la simulación, ya que la velocidad del objeto cambiaría de forma no deseada.

**Diferencia entre paso por valor y paso por referencia**. 

* El paso por valor crea una copia independiente del valor original para no afectarlo y se usa para datos de tipo primitivos (número, booleanos, string, etc...), sin embargo, si queremos pasar un vector (objeto en p5.js) por valor se debe crear una copia del mismo usando **copy()** como el ejemplo anterior en la primera línea. Cuando queremos pasar un dato **por referencia** se copia la referencia en memoria y al modificarla afecta a la variable original por lo que suele usarse para objetos y arrays. En resumen, si asignas un objeto a otra variable ambas apuntaran al mismo objeto en memoria. Si queremos una copia independiente, se crea una nueva instancia con **.copy()** para no modificar la variable original.
 
