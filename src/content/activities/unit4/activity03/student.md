- Una aplicacion que reciba informacion de la camara y cambie la gama de color de la imagen.
- Utiliza el modulo de captura y envio de informacion por medio de la camara web para ser interpretada y modificada por la aplicacion.
- Es una forma de crear un software (pedazo de codigo) que genere el arte basandose en un input.
- primero se debe capturar la imagen de la camara, luego se altera y modifica la imagen recibida por el programa para ser despues mostrada como output.
- Adiciona el código y enlaces necesarios para replicar la aplicación propuesta.

[referencia](https://editor.p5js.org/AidanNelson/sketches/8EcgJpEUi)

```js
/*
This sketch uses p5LiveMedia to share video and data

*/

let myVideo;
let friends = {};
let myName = "";
let peer;
let myPosition = {
  x: 10,
  y: 10
}

function setup() {
  createCanvas(windowWidth, windowHeight);

  myVideo = createCapture(VIDEO,
    function(stream) {
      peer = new p5LiveMedia(this, "CAPTURE", stream, "friendTown")
      peer.on('stream', gotStream);
      // set up a data channel:
      peer.on('data', gotData);
    
    }
  );
  myVideo.muted = true;
  myVideo.hide();
}

function draw() {
  background(220, 100, 250);
  image(myVideo, myPosition.x,myPosition.y, 160,120);

  // this is one way of iterating over an object
  for (const id in friends){
    friends[id].display();
  }
}

// We got a new stream!
function gotStream(stream, id) {
  friends[id] = new Friend (stream);
}

function gotData(data, id) {
  console.log("Got data:",data,"from:",id);


  // If it is JSON, parse it
  let parsedData = JSON.parse(data);

  
  if (friends[id]){
    friends[id].x = parsedData.x;
    friends[id].y = parsedData.y;
  }

}

function mousePressed() {
  
  myPosition = {
    x: mouseX,
    y: mouseY
  }

  // Have to send string
  peer.send(JSON.stringify(myPosition));
}



class Friend {
  constructor(stream, id) {
    this.x = 0;
    this.y = 0;
    this.stream = stream;
  }

  display() {
    image(this.stream, this.x, this.y, 160, 120);
  }
}
```

Esta parte del codigo se encarga de tomar la senal y mostrarla en un canvas.

```js
let img;
let myCanvas;
let squareIt;
let rollIt;
let bendIt;
let original;

let roll = false;
let selected = "none";

function preload() {
  // img = loadImage(
  //   "https://th.bing.com/th/id/OIG.aNqD2DLYf_snrEaj01eu?pid=ImgGn"
  // );
img = loadImage('./assets/ada.png');
}

function setup() {
  frameRate(20);
  img.resize(600, 600);
  myCanvas = createCanvas(img.width, img.height);

  original = createButton("Original");
  original.position(10, img.height + 10);
  original.mousePressed(originalImage);

  squareIt = createButton("Square It");
  squareIt.position(80, img.height + 10);
  squareIt.mousePressed(squareItImage);

  rollIt = createButton("Roll It");
  rollIt.position(160, img.height + 10);
  rollIt.mousePressed(rollItImage);

  bendIt = createButton("Bend It");
  bendIt.position(220, img.height + 10);
  bendIt.mousePressed(bendItImage);

  anime = createButton(roll ? "Stop" : "Animate");
  anime.position(320, img.height + 10);
  anime.mousePressed(animate);

  for (let col = 0; col < img.width; col += 1) {
    for (let row = 0; row < img.height; row += 1) {
      let c = img.get(col, row);
      stroke(color(c));
      point(col, row);
    }
  }
}

function originalImage() {
  background(0);
  for (let col = 0; col < img.width; col += 1) {
    for (let row = 0; row < img.height; row += 1) {
      let c = img.get(col, row);
      stroke(color(c));
      strokeWeight(10);
      point(col, row);
    }
  }
}

function rollItImage() {
  selected = "circle";
  background(0);
  for (let col = 0; col < img.width; col += 10) {
    for (let row = 0; row < img.height; row += 10) {
      let c = img.get(col, row);
      stroke(color(c));
      strokeWeight(2);
      c[3] = random(255); // add alpah
      fill(c);
      ellipse(col, row, random(1, 9));
    }
  }
}

function squareItImage() {
  selected = "cube";
  background(0);
  for (let col = 0; col < img.width; col += 10) {
    for (let row = 0; row < img.height; row += 10) {
      let c = img.get(col, row);
      stroke(color(c));
      strokeWeight(2);
      c[3] = random(255); // add alpah
      fill(c);
      rect(
        col + random(-5, 5),
        row + random(-5, 5),
        random(-10, 10),
        random(-10, 10)
      );
    }
  }
}

function bendItImage() {
  selected = "bend";
  background(0);
  for (let col = 0; col < img.width; col += 10) {
    for (let row = 0; row < img.height; row += 10) {
      let c = img.get(col, row);
      stroke(color(c));
      strokeWeight(random(0, 2));
      c[3] = random(255); // add alpah
      fill(c);
      arc(col, row, random(1, 20), random(1, 20), PI + QUARTER_PI, TWO_PI);
    }
  }
}

function animate() {
  roll = roll === true ? false : true;
  anime.html(roll ? "Stop" : "Animate");
}

function draw() {
  if (roll) {
    switch (selected) {
      case "cube":
        squareItImage();
        break;
      case "circle":
        rollItImage();
        break;
      case "bend":
        bendItImage();
        break;
      default:
        break;
    }
  }
}

function keyPressed() {
  if (key === 's') {
    saveGif('cosmy', 5);
  }
}
```

Esta parte del codigo sirve para editar la imagen.
