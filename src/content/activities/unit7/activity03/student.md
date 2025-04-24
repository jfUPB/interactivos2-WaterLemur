**server.js**

```js
io.on('connection', (socket) => {
    console.log('New client connected');
    socket.on('message', (message) => {
        console.log(`Received message => ${message}`);
        socket.broadcast.emit('message', message);

        if (posX > lastPosX){
            soundAggressiveness = posX - lastPosX;
          }
          else if(posX < lastPosX){
            soundAggressiveness = posX - lastPosX;
          }
          if (posY > lastPosY){
            increaseAudio();
          }
          else if (posY < lastPosY){
            decreaseAudio();
          }
    });

    socket.on('disconnect', () => {
        console.log('Client disconnected');
    });
});
```

**sketch.js**

```
function processData(){
    if (socket && socket.connected) { 

      let sendData = {
        type: 'pos',
        x: posX,
        y: posY
      };

      socket.emit('message', JSON.stringify(touchData));
  }
```

El cliente recibe los datos del mundo real y los envía al servidor para ser procesados y modificar el output.
