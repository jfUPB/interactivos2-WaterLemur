##### Desktop sketch.js

```js
let socket;
let fileInput;
 
function setup() {
  createCanvas(400, 400);
  background(220);
 
  // Conectar con el servidor socket.io
  socket = io();
 
  // Crear el input para cargar archivos
  fileInput = createFileInput(handleFile);
  fileInput.position(10, 10);
 
  // Texto instructivo en el canvas
  textSize(16);
  fill(0);
  text('Carga una imagen para enviarla', 10, 50);
}
 
function handleFile(file) {
  // Verifica que el archivo sea una imagen
  if (file.type === 'image') {
    // (Opcional) Mostrar la imagen localmente
    let img = createImg(file.data, '');
    img.hide();
   
    // Enviar la imagen al servidor (en formato Data URL)
    socket.emit('message', file.data);
    console.log('Imagen enviada');
  } else {
    console.log('El archivo seleccionado no es una imagen');
  }
}
```

##### Mobile sketch.js

```js
let socket;
let receivedImg;
 
function setup() {
  createCanvas(400, 400);
  background(220);
 
  // Conectar con el servidor socket.io
  socket = io();
 
  // Escuchar el evento 'message' para recibir imágenes
  socket.on('message', function(data) {
    console.log('Imagen recibida');
    // Cargar la imagen recibida (data URL) y asignarla a la variable global
    loadImage(data, img => {
      receivedImg = img;
    });
  });
}
 
function draw() {
  background(220);
  if (receivedImg) {
    // Mostrar la imagen recibida en el canvas
    image(receivedImg, 0, 0, width, height);
  } else {
    fill(0);
    textSize(16);
    textAlign(CENTER, CENTER);
    text('Esperando imagen...', width / 2, height / 2);
  }
}
```

- Se conectan por medio de la funcion **Socket.IO**
- Se comunican por medio del servidor, el cual sirve como intermediario para recivir y enviar los datos.
- Se envian mensajes con cabecera y cuerpo, indicando el tipo de mensaje y su contenido.
- Se envian los archivos de imagen serializados.
- Pedir al usuario la imagen a enviar, el envio de la imagen, la escucha para la reception de esta.
- El servidor esta a la escucha de mensajes de los clientes, esperando a que estos se comuniquen con el.
- Los clientes se comunican por intermedio del servidor.
