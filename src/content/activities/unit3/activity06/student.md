##### Codigo del servidor (Flask)

```py
from flask import Flask, request, jsonify
import os
import requests

app = Flask(__name__)

# Directorio donde se guardarán las imágenes temporalmente
UPLOAD_FOLDER = 'uploads'
os.makedirs(UPLOAD_FOLDER, exist_ok=True)
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

# Dirección del Cliente 2
CLIENT_2_URL = "http://cliente2_ip:5001/upload"  # Cambiar con la IP del Cliente 2

# Ruta para recibir la imagen
@app.route('/upload', methods=['POST'])
def upload_file():
    if 'file' not in request.files:
        return jsonify({'error': 'No file part'}), 400
    file = request.files['file']
    if file.filename == '':
        return jsonify({'error': 'No selected file'}), 400
    if file:
        # Guardar la imagen temporalmente en el servidor
        filepath = os.path.join(app.config['UPLOAD_FOLDER'], file.filename)
        file.save(filepath)

        # Enviar la imagen al Cliente 2
        with open(filepath, 'rb') as f:
            files = {'file': (file.filename, f, 'image/jpeg')}
            response = requests.post(CLIENT_2_URL, files=files)

        if response.status_code == 200:
            return jsonify({'message': 'File uploaded and forwarded to Client 2'}), 200
        else:
            return jsonify({'error': 'Failed to forward file to Client 2'}), 500

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

```

##### Codigo del Cliente 1

```py
import tkinter as tk
from tkinter import filedialog
import requests

def select_file():
    filepath = filedialog.askopenfilename(title="Seleccionar una imagen")
    if filepath:
        send_image(filepath)

def send_image(filepath):
    url = 'http://localhost:5000/upload'  # Dirección del servidor
    with open(filepath, 'rb') as file:
        files = {'file': (filepath, file, 'image/jpeg')}
        response = requests.post(url, files=files)
        if response.status_code == 200:
            print("Imagen enviada con éxito.")
        else:
            print("Error al enviar la imagen:", response.json())

# Crear la ventana principal
root = tk.Tk()
root.title("Enviar Imagen")

# Botón para seleccionar la imagen
button = tk.Button(root, text="Seleccionar Imagen", command=select_file)
button.pack(pady=20)

# Iniciar la interfaz gráfica
root.mainloop()
```

##### Codigo del cliente 2

```py
from flask import Flask, request, jsonify
import os

app = Flask(__name__)

# Directorio donde se guardarán las imágenes
UPLOAD_FOLDER = 'received_images'
os.makedirs(UPLOAD_FOLDER, exist_ok=True)
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

# Ruta para recibir la imagen
@app.route('/upload', methods=['POST'])
def upload_file():
    if 'file' not in request.files:
        return jsonify({'error': 'No file part'}), 400
    file = request.files['file']
    if file.filename == '':
        return jsonify({'error': 'No selected file'}), 400
    if file:
        # Guardar la imagen recibida
        filepath = os.path.join(app.config['UPLOAD_FOLDER'], file.filename)
        file.save(filepath)
        return jsonify({'message': 'File received successfully', 'filepath': filepath}), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5001)
```


- Los clientes se comunican con el servidor mediante la funcion request y post

```py
response = requests.post(url, files=files)
        if response.status_code == 200:
            print("Imagen enviada con éxito.")
```
```py
 file = request.files['file']
```

- Los clientes se comunican entre si mediante el servidor.

- Se envia la direccion donde se almacena la imagen.

- Se envian mensajes en formato json.

- Se generan eventos de tipo trigger, se consumen una sola vez.

- Solo se envia la direccion donde se almacena la imagen.

- Los clientes no intercambian datos directamente, lo hacen por medio del servidor.
