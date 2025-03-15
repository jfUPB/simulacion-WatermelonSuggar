![image](https://github.com/user-attachments/assets/7ee4b72a-6a1e-41b1-8575-d41f05f0174d)

Enlace a la simulación [aquí](https://editor.p5js.org/WatermelonSuggar/sketches/URUd4ztIU)

```js
let vehicle;

function setup() {
  createCanvas(600, 400);
  vehicle = new Vehicle(width / 2, height - 50);
}

function draw() {
  // Dibujar el fondo
  background(135, 206, 235); // Cielo azul
  
  // Dibujar el sol
  fill(255, 204, 0);
  noStroke();
  ellipse(80, 80, 60, 60);
  
  // Dibujar montañas
  fill(16, 165, 23);
  triangle(0, 400, 200, 100, 500, 400);
  triangle(300, 400, 500, 150, 700, 400);


  
  // Dibujar la carretera
  fill(50);
  rect(0, height - 40, width, 40);
  
  // Dibujar líneas de la carretera
  stroke(255);
  strokeWeight(4);
  for (let i = 0; i < width; i += 40) {
    line(i, height - 20, i + 20, height - 20);
  }
  
  vehicle.update();
  vehicle.edges();
  vehicle.display();
}

function keyPressed() {
  if (keyCode === LEFT_ARROW) {
    vehicle.applyForce(createVector(-0.5, 0));
  } else if (keyCode === RIGHT_ARROW) {
    vehicle.applyForce(createVector(0.5, 0));
  }
}

class Vehicle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.topspeed = 6;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Reset acceleration each frame
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
  }

  display() {
    let angle = this.velocity.heading();
    
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    fill(21, 16, 165 );
    stroke(255);
    strokeWeight(2);
    
    // Cuerpo del carro (triángulo estilizado)
    beginShape();
    vertex(20, 0);
    vertex(-15, -10);
    vertex(-15, 10);
    endShape(CLOSE);
    
    // Ruedas
    fill(50);
    
    pop();
  }
}


```
