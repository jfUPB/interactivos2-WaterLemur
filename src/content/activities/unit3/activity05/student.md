**Experimento 2**

Al cambiar el string 'message' por 'test', se interfiere la comunicacion porque no hay codigo que interprete ese tipo de mensaje.

```js
function touchMoved() {
    if (socket && socket.connected) { 
        let dx = abs(mouseX - lastTouchX);
        let dy = abs(mouseY - lastTouchY);

        if (dx > threshold || dy > threshold) {
            let touchData = {
                type: 'touch',
                x: mouseX,
                y: mouseY
            };
            socket.emit('test', JSON.stringify(touchData));

            lastTouchX = mouseX;
            lastTouchY = mouseY;
        }
    }
    return false;
```

**Experimento 3**

Los datos enviados los dividi por 1.5

```js
function touchMoved() {
    if (socket && socket.connected) { 
        let dx = abs(mouseX - lastTouchX);
        let dy = abs(mouseY - lastTouchY);

        if (dx > threshold || dy > threshold) {
            let touchData = {
                type: 'touch',
                x: mouseX / 1.5,
                y: mouseY / 1.5
            };
            socket.emit('message', JSON.stringify(touchData));

            lastTouchX = mouseX;
            lastTouchY = mouseY;
        }
    }
    return false;
```

**Experimento 4**

```js
El codigo del desktop solo funciona cuando el tipo de dato es 'touch'

    // Recibir mensaje del servidor
    socket.on('message', (data) => {
        console.log(`Received message: ${data}`);
        let parsedData = JSON.parse(data);
        if (parsedData.type === 'touch') {
            circleX = parsedData.x;
            circleY = parsedData.y;
        }
    });
```

Aprendi que la comunicacion entre los dos sistemas debe ser claro y entendible para ambos, lo que se envia debe tener una correspondiente interpretacion
