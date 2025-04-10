##### "Blueprint Algorítmico - Unidad 6".
  *   **Resumen del concepto y narrativa:** El proyecto es una propuesta artistica que convierte la posicion del usuario en musica.
  *   **Inputs detallados:**
       | Input         | Tipo    | Rango      | Simulación | Storytelling            |
       |---------------|---------|------------|------------|-------------------------|
       | Acelerómetro | float   | 0.0-1.0    | No         | La posición como sonido |
       | Pulsador      | boolean | true-false | No         | Cambio de ambientación  |

      1.  **Identifica inputs a simular:** El botón que cambia la canción.
      2.  **Define método de simulación:**
          *   ¿Usarás `random()`? No.
          *   ¿Usarás `noise()` No.
          *   ¿Crearás sliders o botones simples en pantalla para controlar manualmente el valor simulado durante las pruebas? No.
          *   ¿Seguirás alguna curva o patrón predefinido? No.
      3.  **Comportamiento simulado:** Se esperan cambios lentos y consistentes.

      4.  **(Opcional) Activación/Desactivación:** Seria útil una variable para cambiar los sonidos aleatoriamente para las pruebas.
  *   **Proceso (algoritmo):**

      -  Se recivira como *input* la posicion del usuario, esta sera en una medicion de 0.0 a 1.0, alterando el sonido producido dependiendo del valor.

      ![diagrama](../../../../assets/diagrama-ipo.jpg)

      -   **Pseudocodigo**

      ```
      SI input_movimiento > umbral ENTONCES aumentar_agresividad
      SI input_movimiento < umbral ENTONCES aumentar_pasividad
      SI input_movimiento = 0 ENTONCES   no_hacer_nada
      ```  
  *  **Conexion con el Storytelling**
     En esta propuesta artistica se pretende transformar el movimiento del usuario en musica, siendo el usuario, que con cada paso editara el sonido personalizado.
  *   **Outputs detallados:** 
      -   El tipo principal es sonoro.
          -   El sonido dependerá de la posición del usuario, el sonido será más ameno cuando el usuario se mueve a la izquierda y más agresivo cuando se mueva hacia la derecha. Será más fuerte cuando el usuario se mueva hacia adelante y más suave cuando se mueva para atrás.
          -   Se recogerá la posición del usuario como input y esta será transformada para producir un sonido único, un cambio en la posición y el sonido cambiara de tono, de ameno a agresivo y viceversa
          -   La posición del usuario *"contara"* una historia a medida que se mueva, creando una pieza de audio única
