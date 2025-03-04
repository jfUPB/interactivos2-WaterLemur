##### Codigo del servidor

```js
const express = require('express');
const http = require('http');
const socketIo = require('socket.io');
const multer = require('multer');
const path = require('path');
const app = express();
const server = http.createServer(app);
const io = socketIo(server);

// Setup multer for file uploads
const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'uploads/');
    },
    filename: (req, file, cb) => {
        cb(null, file.originalname);
    }
});
const upload = multer({ storage: storage });

// Serve the HTML file
app.get('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'index.html'));
});

// Handle file upload
app.post('/upload', upload.single('image'), (req, res) => {
    const filePath = req.file.path;
    io.emit('imageUploaded', filePath); // Emit the file path to all connected clients
    res.sendStatus(200);
});

// Set up Socket.IO
io.on('connection', (socket) => {
    console.log('A user connected');
    socket.on('disconnect', () => {
        console.log('A user disconnected');
    });
});

// Start server
const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

##### Front End

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image Upload</title>
    <script src="/socket.io/socket.io.js"></script>
</head>
<body>
    <h1>Upload an Image</h1>
    <form id="uploadForm" enctype="multipart/form-data">
        <input type="file" name="image" accept="image/*" required>
        <button type="submit">Upload</button>
    </form>
    <h2>Uploaded Images</h2>
    <div id="images"></div>

    <script>
        const socket = io();
        
        const form = document.getElementById('uploadForm');
        form.addEventListener('submit', (event) => {
            event.preventDefault();

            const formData = new FormData(form);
            fetch('/upload', {
                method: 'POST',
                body: formData
            });
        });

        socket.on('imageUploaded', (filePath) => {
            const imagesDiv = document.getElementById('images');
            const img = document.createElement('img');
            img.src = filePath; // You might need to serve this URL correctly
            img.style.height = '200px';
            img.style.margin = '10px';
            imagesDiv.appendChild(img);
        });
    </script>
</body>
</html>
```

- Se conectan por medio de la funcion **Socket.IO**
- Se comunican por medio del servidor, el cual sirve como intermediario para recivir y enviar los datos.
- Se envian mensajes con cabecera y cuerpo, indicando el tipo de mensaje y su contenido.
- Se envian los archivos de imagen serializados.
- Pedir al usuario la imagen a enviar, el envio de la imagen, la escucha para la reception de esta.
- El servidor esta a la escucha de mensajes de los clientes, esperando a que estos se comuniquen con el.
- Los clientes se comunican por intermedio del servidor.
