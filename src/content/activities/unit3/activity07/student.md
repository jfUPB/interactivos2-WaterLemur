- Aprendi a instalar dependencias desde la consola, tambien aprendi mas sobre el concepto de cliente-servidor.
- No tube mayores dificultades al entender temas
- Se aplicaron los conceptos del modelo cliente servidor, los clientes se comunican por medio del servidor que sirve como intermediario en la comunicacion.


```js
// Set up Socket.IO
io.on('connection', (socket) => {
    console.log('A user connected');
    socket.on('disconnect', () => {
        console.log('A user disconnected');
    });
});
```

```js
// Handle file upload
app.post('/upload', upload.single('image'), (req, res) => {
    const filePath = req.file.path;
    io.emit('imageUploaded', filePath); // Emit the file path to all connected clients
    res.sendStatus(200);

```

```js
        socket.on('imageUploaded', (filePath) => {
            const imagesDiv = document.getElementById('images');
            const img = document.createElement('img');
            img.src = filePath; // You might need to serve this URL correctly
            img.style.height = '200px';
            img.style.margin = '10px';
            imagesDiv.appendChild(img);
```

Los clientes envian las imagenes al servidor y luego este se comunica con los clientes.

- Se debe tener un buen manejo del lenguaje javascript pero en general no parece muy complicado.

- Entender bien el paradigma de javascript para poder escribir el codigo fluidamente.
