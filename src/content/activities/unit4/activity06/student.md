**Simulación inspirada en el juego Flappy Bird**

![image](https://github.com/user-attachments/assets/50e65fa4-4b69-4ba1-a634-9fd07673d358)

Simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/auirvBj3T)

**El código**

```js
let bird;
let pipes = [];
let gravity = 0.5;
let lift = -10;
let gameOver = false;

function setup() {
  createCanvas(400, 600);
  bird = new Bird();
  pipes.push(new Pipe());
}

function draw() {
  background(135, 206, 250);
  
  if (!gameOver) {
    bird.update();
    bird.show();
    
    if (frameCount % 100 === 0) {
      pipes.push(new Pipe());
    }
    
    for (let i = pipes.length - 1; i >= 0; i--) {
      pipes[i].update();
      pipes[i].show();
      
      if (pipes[i].offscreen()) {
        pipes.splice(i, 1);
      }
      
      if (pipes[i].hits(bird)) {
        gameOver = true;
      }
    }
    
    if (bird.y >= height) {
      gameOver = true;
    }
  } else {
    showGameOverScreen();
  }
}

function keyPressed() {
  if (key === ' ') {
    if (!gameOver) {
      bird.up();
    } else {
      resetGame();
    }
  }
}

function showGameOverScreen() {
  fill(0, 0, 0, 150);
  rect(0, 0, width, height);
  fill(255);
  textSize(32);
  textAlign(CENTER, CENTER);
  text("Game Over", width / 2, height / 3);
  textSize(16);
  text("Presiona ESPACIO para reiniciar", width / 2, height / 2);
}

function resetGame() {
  bird = new Bird();
  pipes = [];
  pipes.push(new Pipe());
  gameOver = false;
}

class Bird {
  constructor() {
    this.x = 50;
    this.y = height / 2;
    this.velocity = 0;
  }

  up() {
    this.velocity = lift;
  }

  update() {
    this.velocity += gravity;
    this.velocity *= 0.9;
    this.y += this.velocity;
    
    if (this.y > height) {
      this.y = height;
      this.velocity = 0;
    }
    if (this.y < 0) {
      this.y = 0;
      this.velocity = 0;
    }
  }

  show() {
    fill(255, 204, 0);
    ellipse(this.x, this.y, 30, 30);
  }
}

class Pipe {
  constructor() {
    this.x = width;
    this.w = 50;
    this.gap = 150;
    this.amplitude = 50;
    this.frequency = 0.02;
    this.offset = random(TWO_PI);
  }

  update() {
    this.x -= 3;
  }

  show() {
    let centerY = height / 2 + this.amplitude * sin(frameCount * this.frequency + this.offset);
    let top = centerY - this.gap / 2;
    let bottom = centerY + this.gap / 2;
    
    fill(34, 139, 34);
    rect(this.x, 0, this.w, top);
    rect(this.x, bottom, this.w, height - bottom);
  }

  offscreen() {
    return this.x + this.w < 0;
  }

  hits(bird) {
    let centerY = height / 2 + this.amplitude * sin(frameCount * this.frequency + this.offset);
    let top = centerY - this.gap / 2;
    let bottom = centerY + this.gap / 2;
    
    if (bird.x > this.x && bird.x < this.x + this.w) {
      if (bird.y < top || bird.y > bottom) {
        return true;
      }
    }
    return false;
  }
}
```


Enlace a la simulación en el editor de p5.js.
Código de la simulación.
Captura de pantalla de la simulación.
