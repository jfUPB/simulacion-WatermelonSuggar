


Explica brevemente cómo configuraste p5.sound y qué datos de audio estás extrayendo.
Muestra fragmentos de código clave donde los datos del audio modulan tu algoritmo generativo y donde el algoritmo controla los outputs visuales.
Incluye el código completo de tu sketch final (o enlace a repositorio).

**Código**

flowField.js

```js
class FlowField {
  constructor(resolution) {
    this.resolution = resolution;
    this.cols = floor(width  / resolution);
    this.rows = floor(height / resolution);
    this.field = new Array(this.cols * this.rows);
    this.zoff  = 0;
  }

  // bassEnergy y midEnergy afectan escala y torsión
  update(bassEnergy, midEnergy) {
    const scaleBass = map(bassEnergy, 0, 255, 0.01, 0.08);
    const twistMid  = map(midEnergy, 0, 255, 0, 3);

    let xoff = 0;
    for (let x = 0; x < this.cols; x++) {
      let yoff = 0;
      for (let y = 0; y < this.rows; y++) {
        const angle = noise(xoff, yoff, this.zoff) * TWO_PI * (2 + twistMid);
        this.field[x + y * this.cols] = p5.Vector.fromAngle(angle);
        yoff += scaleBass;
      }
      xoff += scaleBass;
    }
    this.zoff += 0.001;
  }

  lookup(pos) {
    const col = floor(constrain(pos.x / this.resolution, 0, this.cols - 1));
    const row = floor(constrain(pos.y / this.resolution, 0, this.rows - 1));
    return this.field[col + row * this.cols].copy();
  }
}
```

glyph.js

```js
class Glyph {
  constructor(x, y) {
    this.pos  = createVector(x, y);
    this.life = 120;
    this.size = random(16, 32);
  }

  update() { this.life--; }

  display(fgColor) {
    push();
    translate(this.pos.x, this.pos.y);
    noFill();
    stroke(fgColor[0], fgColor[1], fgColor[2], this.life * 2);
    rotate(frameCount * 0.02);

    beginShape();
    for (let a = 0; a < TWO_PI * 3; a += 0.1) {
      const r = map(a, 0, TWO_PI * 3, 0, this.size);
      vertex(cos(a) * r, sin(a) * r);
    }
    endShape();
    triangle(-this.size, this.size * 0.3, this.size, this.size * 0.3, 0, -this.size);
    pop();
  }
}
```
particle.js

```js
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector();
    this.lifespan = 255;
    this.baseSize = random(4, 8);
    this.size     = this.baseSize;
    this.deform   = 0;
    this.type = 0;          // 0=círculo, 1=triángulo, 2=blob
  }

  applyForce(f) { this.acc.add(f); }

  // recibe waveform
  update(ampLvl, midEnergy, waveArr) {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;

    // pulsación global
  let targetSize = map(ampLvl, 0, 1, this.baseSize * 0.5, this.baseSize * 3);
this.size = lerp(this.size, targetSize, 0.1); // suaviza la transición de tamaño


    // deformación geométrica
    this.deform = map(midEnergy, 0, 255, 0, 0.6) * map(mouseY, 0, height, 0, 1);

    // ondulación local con waveform
    const idx = floor(map(this.pos.x, 0, width, 0, waveArr.length - 1));
    const waveBoost = map(waveArr[idx], -1, 1, -0.3, 0.3);
    this.size *= 1 + waveBoost;
  }

  display(fgColor) {
    noFill();
    stroke(fgColor[0], fgColor[1], fgColor[2], max(this.lifespan, 60));
    strokeWeight(map(this.lifespan, 255, 0, 0.5, 2));

    push();
    translate(this.pos.x, this.pos.y);
    rotate(frameCount * 0.01);

    switch (this.type) {
      case 0:
        ellipse(0, 0, this.size * (1 + this.deform));
        break;
      case 1:
        polygon(0, 0, this.size, 3, this.deform);
        break;
      case 2:
        fractalBlob(this.size, this.deform);
        break;
    }
    pop();
  }

  isDead() { return this.lifespan < 0; }

  edges() {
    if (this.pos.x > width)  this.pos.x = 0;
    if (this.pos.x < 0)      this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0)      this.pos.y = height;
  }
}

/* === helpers === */
function polygon(x, y, radius, npoints, deform = 0) {
  const angle = TWO_PI / npoints;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    const r = radius * (1 + deform * noise(a));
    vertex(x + cos(a) * r, y + sin(a) * r);
  }
  endShape(CLOSE);
}

function fractalBlob(radius, deform) {
  beginShape();
  for (let a = 0; a < TWO_PI; a += 0.2) {
    const offset = map(noise(cos(a) + 1, sin(a) + 1), 0, 1, -deform, deform);
    const r = radius * (1 + offset);
    vertex(cos(a) * r, sin(a) * r);
  }
  endShape(CLOSE);
}
```
particleSystem.js

```js
class ParticleSystem {
  constructor(flowField) {
    this.flowField = flowField;
    this.particles = [];
  }

  // recibe waveform
  update(ampLvl, mid, treble, peakHit, waveArr) {
    /* spawn controlado por agudos */
    const spawnCount = int(map(treble, 0, 255, 0, 30));
    for (let i = 0; i < spawnCount; i++) {
      this.particles.push(new Particle(random(width), random(height)));
    }

    /* destello extra en picos */
    if (peakHit) {
      for (let i = 0; i < 20; i++) {
        this.particles.push(new Particle(random(width), random(height)));
      }
    }

    /* actualizar y dibujar */
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const p = this.particles[i];
      p.type = floor(map(mouseX, 0, width, 0, 2.99));

      const force = this.flowField.lookup(p.pos).mult(0.2);
      p.applyForce(force);
      p.update(ampLvl, mid, waveArr);
      p.edges();

      if (p.isDead()) this.particles.splice(i, 1);
    }
  }

  display(fgColor) {
    for (let p of this.particles) p.display(fgColor);
  }
}
```

roots.js

```js
class Roots {
  constructor() {
    this.radius  = 0;
    this.strokeW = 1;
    this.spec    = [];
  }

  update(bassEnergy, spec) {
    this.radius  = map(bassEnergy, 0, 255, 20, max(width, height) * 0.6);
    this.strokeW = map(bassEnergy, 0, 255, 0.2, 3);
    this.spec    = spec;   // guarda el espectro de 512 bins
  }

  display(fgColor) {
    push();
    translate(width / 2, height / 2);
    strokeWeight(this.strokeW);
    stroke(fgColor[0], fgColor[1], fgColor[2], 255);   // alpha fijo p/debug

    const segments = 64;
    for (let i = 0; i < segments; i++) {
      const angle = TWO_PI * i / segments;
      const bin   = floor(map(i, 0, segments - 1, 0, this.spec.length - 1));
      const amp   = this.spec[bin];                    // 0-255
      const len   = map(amp, 0, 255, this.radius * 0.2, this.radius);
      line(0, 0, cos(angle) * len, sin(angle) * len);
    }
    pop();
  }
}
```
sketch.js

```js
const smoothing = 0.5;
const bins      = 512;

let song, amp, fft;
let waveform = [];
let spectrum = [];
let volLow, volMid, volHigh;

let flowField, roots, ps;
let glyphs = [];

/* ─── Paletas de color personalizadas ─── */
const palettes = [
  // Paleta 0 – tonos púrpura y cian apagados
  { bg: [15, 10, 25], fg: [ 90,  20, 140] },

  // Paleta 1 – rojos profundos y dorados oscuros
  { bg: [20,  5,  5], fg: [200, 90,  40] },

  // Paleta 2 – verdes sombríos (si quieres agregar más)
  { bg: [ 5, 15, 10], fg: [ 30,120, 60] }
];
let palIndex = 0;   // arrancar con la primera


function preload() {
  song = loadSound('TheEmptinessMachine.mp3');   // 
}

function setup() {
  createCanvas(600, 600);
  noCursor();

  amp = new p5.Amplitude();
  fft = new p5.FFT(smoothing, bins);

  flowField = new FlowField(25);
  roots     = new Roots();
  ps        = new ParticleSystem(flowField);
}

function draw() {
  /* ───── 1. ANÁLISIS DE AUDIO ───── */
  fft.analyze();
  waveform = fft.waveform();
  spectrum = fft.analyze();            // 512-bins

  volLow  = fft.getEnergy(20, 250);    // graves ampliados
  volMid  = fft.getEnergy(250, 2000);  // medios
  volHigh = fft.getEnergy(2000, 8000); // agudos

  const ampLvl = amp.getLevel();

  /* ───── 2. FONDO (siempre en BLEND) ───── */
  blendMode(BLEND);                   
  const alpha = map(ampLvl, 0, 1, 200, 20);   
  const bg    = palettes[palIndex].bg;
  background(bg[0], bg[1], bg[2], alpha);

  /* (Opcional) reactiva modo aditivo para brillos */
  blendMode(ADD);

  /* ───── 3. SISTEMAS VISUALES ───── */
  flowField.update(max(volLow, 10), max(volMid, 10));   // nunca 0

  roots.update(max(volLow, 10), spectrum);
  roots.display(palettes[palIndex].fg);

  const peakHit = volHigh > 180;
  ps.update(ampLvl, max(volMid, 20), volHigh, peakHit, waveform);
  ps.display(palettes[palIndex].fg);

  /* ───── 4. GLIFOS (clic) ───── */
  for (let g of glyphs) {
    g.update();
    g.display(palettes[palIndex].fg);
  }
  glyphs = glyphs.filter(g => g.life > 0);
}

/* ──────────── INTERACCIONES ──────────── */
function mousePressed() {
  /* 1. Autoplay seguro */
  if (!song.isPlaying()) {
    userStartAudio();   // desbloquea el AudioContext
    song.play();
  }

  /* 2. Revelar símbolo */
  glyphs.push(new Glyph(mouseX, mouseY));
}

function keyPressed() {
  palIndex = (palIndex + 1) % palettes.length;
}
```


Inserta un VIDEO DEMO (si esta vez si, pero trata de optimizar el archivo) (¡esencial!) de tu simulación en acción, CON la música sonando. Debe mostrar la respuesta al audio y la naturaleza generativa.
