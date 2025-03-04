- Aprendi a instalar dependencias desde la consola, tambien aprendi mas sobre el concepto de cliente-servidor.
- No tube mayores dificultades al entender temas
- Se aplicaron los conceptos del modelo cliente servidor, los clientes se comunican por medio del servidor que sirve como intermediario en la comunicacion.


```js
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
```

```js
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

```
Los clientes envian las imagenes al servidor y luego este se comunica con los clientes.

- Se debe tener un buen manejo del lenguaje javascript pero en general no parece muy complicado.

- Entender bien el paradigma de javascript para poder escribir el codigo fluidamente.
