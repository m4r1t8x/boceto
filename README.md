## 4) Experimentos

[cite_start]En esta sección se detalla el comportamiento del robot **e-puck** mediante la implementación de diferentes lógicas de control cinemático en Python[cite: 1]. El controlador permite ejecutar diversas trayectorias según el valor de la variable `DESAFIO`:

* **Línea Recta (Desafío 1):** Movimiento uniforme donde $v_l = v_r$, permitiendo un desplazamiento frontal constante.
* **Giro Constante (Desafíos 2 y 3):** Se aplican velocidades diferenciales fijas para generar trayectorias circulares.
* **Trayectoria Cuadrada (Desafío 4):** Se utiliza un control basado en tiempo (`robot.getTime()`) para alternar entre avanzar en línea recta y girar 90° sobre su propio eje.
* **Trayectoria en "S" (Desafío 5):** El robot alterna radios de giro cada 5 segundos para simular una curva sinuosa.

> **Demostración en Video:**
> A continuación se presenta el video que muestra el funcionamiento del robot en el simulador Webots:
> ![Video del funcionamiento](URL_DE_TU_VIDEO_O_GIF_AQUI)

---

## 5) Análisis de Preguntas

*Nota: Sección pendiente de integración final según los resultados del equipo.*

1.  **¿Cómo influye el radio de la rueda (`wheel_radius`) en el movimiento?**
    La velocidad lineal es proporcional al radio ($v = r \cdot \omega$). Un radio mayor permite recorrer más distancia por cada revolución del motor.
2.  **¿Cuál es la importancia de la distancia entre ruedas (`distance_between_wheels`)?**
    Este parámetro define la cinemática de giro del robot diferencial. Es esencial para calcular el tiempo necesario para rotar ángulos específicos (como los 90° del cuadrado).
3.  **¿Para qué sirve el factor de corrección en los giros?**
    En el código se observa un multiplicador (ej. `1.04`) para la duración del giro. Esto sirve para compensar errores de fricción o deslizamiento dentro del motor de física de Webots y lograr ángulos precisos.

---

## 6) Conclusión

[cite_start]Este laboratorio permitió validar los principios de la **cinemática de robots diferenciales** en un entorno controlado. A través de la programación en Python, se logró:

* Implementar una máquina de estados simple basada en el tiempo para generar trayectorias geométricas.
* Comprender la relación entre la velocidad angular de los motores y el desplazamiento lineal del robot en el plano.
* [cite_start]Aprender a configurar entornos de simulación en **Webots** (arena, luces y sensores) para pruebas de robótica autónoma[cite: 1, 2].

[cite_start]La experiencia destaca la importancia de considerar factores externos (como el tiempo de paso o *timestep*) al diseñar controladores para sistemas dinámicos[cite: 1].
