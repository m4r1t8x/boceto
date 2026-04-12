## Experimentos

En esta sección se documenta el comportamiento del robot e-puck bajo distintos escenarios de control cinemático. 

# Desafíos de Movimiento 
El controlador implementado permite conmutar entre diferentes trayectorias modificando la variable "DESAFIO" en el código:

Línea Recta (Desafío 1): Ambas ruedas giran a la misma velocidad ($v_l = v_r$), permitiendo un desplazamiento frontal uniforme.
Giro Constante (Desafíos 2 y 3): Se aplican velocidades diferenciales para generar trayectorias curvas y giros sobre el eje.
Trayectoria Cuadrada (Desafío 4): Utiliza una máquina de estados basada en el tiempo para alternar entre avance lineal y rotaciones de 90°.
Trayectoria en "S" (Desafío 5): Alterna radios de giro cada 5 segundos para formar una curva sinuosa.

## Análisis de preguntas

## Conclusión
