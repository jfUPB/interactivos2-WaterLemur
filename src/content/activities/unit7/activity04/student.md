**server.js**

Edición del audio basado en los inputs recibidos del cliente.

```js
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
```

```js
function increaseAudio(){
  outputVolume(soundAggressiveness, [rampTime], [timeFromNow])
}
function decreaseAudio(){
  outputVolume(soundAggressiveness, [rampTime], [timeFromNow])
}
```
