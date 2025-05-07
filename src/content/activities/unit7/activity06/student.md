Recibir valor del nuevo volumen:

 ```js
 //Recibir valor del nuevo volumen
  socket.on('audio', (data) => {
    const obj = JSON.parse(data);
    song.volume(obj);
    console.log(`Received audio: ${data}`);
});
 ```

Enviar mensaje al cliente:

 ```js
//Enviar mensaje al cliente
    soundAggressiveness = posX - lastPosX;
    socket.broadcast.emit('audio', JSON.stringify(soundAggressiveness));
 ```
