### Fiesta de partículas

Enlace del proyecto [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/vnKIzoNlA)

**Código**

```js
let img;
let particles = [];
let imgData = [];
let input;
let bgColor; // Color de fondo dinámico
let fiesta = false;
let buttonFiesta, buttonNormal;

function setup() {
  createCanvas(600, 800);
  background(0);
  input = createFileInput(handleFile);
  input.position(10, 10);

  buttonFiesta = createButton('Fiesta');
  buttonFiesta.position(10, 50);
  buttonFiesta.mousePressed(() => toggleFiesta(true));
  
  buttonNormal = createButton('Normal');
  buttonNormal.position(10, 90);
  buttonNormal.mousePressed(() => toggleFiesta(false));

  // Inicializamos el color de fondo en un azul oscuro
  bgColor = createVector(10, 20, 80);
}

function handleFile(file) {
  if (file.type === 'image') {
    img = loadImage(file.data, () => {
      processImage();
    });
  }
}

function processImage() {
  image(img, 0, 0, width, height);
  loadPixels();
  
  imgData = [];
  for (let x = 0; x < width; x += 4) {  // Mayor precisión reduciendo el paso
    for (let y = 0; y < height; y += 4) {
      let index = (x + y * width) * 4;
      let brightness = pixels[index] + pixels[index + 1] + pixels[index + 2];
      if (brightness < 400) {  // Filtrar puntos oscuros de la imagen
        imgData.push(createVector(x, y));
      }
    }
  }
  
  particles = [];
  for (let i = 0; i < imgData.length; i++) {
    particles.push(new Particle(random(width), random(height), imgData[i]));
  }
}

function draw() {
  updateBackground(); // Actualizar el color de fondo con random walk
  background(bgColor.x, bgColor.y, bgColor.z);

  for (let p of particles) {
    p.update();
    p.show();
  }
}

function updateBackground() {
  // Random walk en la gama de azules oscuros
  bgColor.x = constrain(bgColor.x + random(-5, 5), 0, 50);   // Mantener tonos oscuros en R
  bgColor.y = constrain(bgColor.y + random(-5, 5), 0, 50);   // Mantener tonos oscuros en G
  bgColor.z = constrain(bgColor.z + random(-10, 10), 50, 150); // Azul oscuro variable
}

function toggleFiesta(state) {
  fiesta = state;
  if (fiesta) {
    // Cambiar colores a modo fiesta
    for (let p of particles) {
      p.r = random(255);
      p.g = random(255);
      p.b = random(255);
    }
  } else {
    // Restaurar colores y tamaños originales
    for (let p of particles) {
      p.r = constrain(randomGaussian(100, 30), 50, 200);
      p.g = 150;
      p.b = constrain(randomGaussian(255, 20), 200, 255);
      p.size = 3;
    }
  }
}

class Particle {
  constructor(x, y, target) {
    this.pos = createVector(x, y);
    this.target = target;
    this.vel = p5.Vector.random2D();
    this.acc = createVector();
    this.maxSpeed = 2;
    this.size = 3;

    // Distribución normal para el color de las partículas
    this.r = constrain(randomGaussian(100, 30), 50, 200);
    this.g = 150;
    this.b = constrain(randomGaussian(255, 20), 200, 255);
  }
  
  update() {
    if (fiesta) {
      // Movimiento caótico con vuelo de Lévy
      let step = pow(random(1), -1.5);
      let angle = random(TWO_PI);
      this.vel.x = cos(angle) * step;
      this.vel.y = sin(angle) * step;
      this.size = constrain(2 + step * 3, 2, 10); // Variar tamaño con Lévy Flight
    } else {
      let force = p5.Vector.sub(this.target, this.pos);
      force.setMag(0.1);
      this.acc.add(force);
      
      // Reducir gradualmente el tamaño si está en modo normal
      this.size = lerp(this.size, 3, 0.1);
    }
    
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }
  
  show() {
    fill(this.r, this.g, this.b);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.size, this.size);
  }
}



```


Código de la aplicación.
Captura del contenido generado.
En caso de realizar alguna variación al concepto original, escribe un texto donde expliques la razón del cambio.
