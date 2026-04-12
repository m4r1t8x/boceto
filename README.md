## 4) Experimentos

En esta sección se detalla el comportamiento del robot **e-puck** mediante la implementación de diferentes lógicas de control cinemático en Python. El controlador permite ejecutar diversas trayectorias según el valor de la variable `DESAFIO`:

* **Línea Recta (Desafío 1):** Movimiento uniforme donde $v_l = v_r$, permitiendo un desplazamiento frontal constante.
* **Giro Constante (Desafíos 2 y 3):** Se aplican velocidades diferenciales fijas para generar trayectorias circulares.
* **Trayectoria Cuadrada (Desafío 4):** Se utiliza un control basado en tiempo (`robot.getTime()`) para alternar entre avanzar en línea recta y girar 90° sobre su propio eje.
* **Trayectoria en "S" (Desafío 5):** El robot alterna radios de giro cada 5 segundos para simular una curva sinuosa.

> **Demostración en Video:**
> A continuación se presenta el video que muestra el funcionamiento del robot en el simulador Webots:
> ![Video del funcionamiento](URL_DE_TU_VIDEO_O_GIF_AQUI)

---

## 5) Análisis de Preguntas

A continuación se presenta el análisis detallado de los experimentos cinemáticos realizados con el robot e-puck, integrando los fundamentos teóricos y los resultados observados en el simulador.

### 2.1 Modelo Cinemático del Robot Diferencial
[cite_start]El estado del robot se describe mediante el vector de pose $(x, y, \theta)$, donde la orientación $\theta$ cambia según la relación entre las velocidades de las ruedas[cite: 5]. Los parámetros físicos clave utilizados en los cálculos son:
* [cite_start]**Radio de la rueda ($r$):** 0.0205 m[cite: 4, 5].
* [cite_start]**Distancia entre ruedas ($L$):** 0.052 m[cite: 4, 5].

---

### 5.1 ¿Qué ocurre cuando ambas ruedas tienen la misma velocidad?
[cite_start]Cuando $v_r = v_l$, la diferencia de velocidades es nula, lo que implica que la velocidad angular $\omega$ es cero[cite: 5].
* [cite_start]**Análisis:** Al no haber rotación, el radio de curvatura $R$ tiende a infinito[cite: 5].
* [cite_start]**Resultado:** El robot mantiene una trayectoria rectilínea perfecta con una orientación $\theta$ constante[cite: 5].
* [cite_start]**Evidencia:** Confirmado en el **Desafío 1**, donde el robot avanzó sin desviaciones laterales[cite: 5].

### 5.2 ¿Cómo cambia la trayectoria cuando las velocidades son diferentes?
[cite_start]Al existir una diferencia entre $v_r$ y $v_l$, el robot describe un arco circular cuyo radio depende de dicha disparidad[cite: 5].
* [cite_start]**Análisis:** La curvatura es directamente proporcional a la diferencia de velocidades[cite: 5]. [cite_start]La rueda con menor velocidad determina el interior del giro[cite: 5].
* **Resultados Experimentales:**
    * [cite_start]**Desafío 2 (Diferencia del 10%):** Se observó un giro continuo con un radio amplio calculado de $R \approx 0.49$ m[cite: 5].
    * [cite_start]**Desafío 3 ($v_l=2.0, v_r=5.0$):** Se generó un círculo mucho más cerrado con un radio de $R \approx 0.06$ m[cite: 5].

### 5.3 ¿Qué ocurre cuando una rueda gira en sentido opuesto a la otra?
[cite_start]Si se aplican velocidades de igual magnitud pero signos opuestos ($v_r = -v_l$), la velocidad lineal resultante $v$ es cero[cite: 5].
* [cite_start]**Análisis:** El robot realiza una **rotación pura** sobre su propio eje central[cite: 5].
* [cite_start]**Aplicación:** Esta maniobra permite cambiar la orientación $\theta$ sin alterar la posición $(x, y)$[cite: 5].
* [cite_start]**Evidencia:** Esta lógica fue fundamental para el **Desafío 4**, permitiendo que el robot girara exactamente 90° en las esquinas del cuadrado[cite: 5].

### 5.4 ¿Qué tipo de movimiento permite dibujar un círculo?
[cite_start]Para trazar un círculo perfecto y constante, se requiere que la relación de velocidades sea fija y del mismo signo ($v_r/v_l = \text{constante} \neq 1$)[cite: 5].
* [cite_start]**Condición:** Las velocidades deben permanecer constantes en el tiempo para que el radio $R$ y la velocidad angular $\omega$ no varíen[cite: 5].
* **Tiempo de Ciclo:** El tiempo para completar una revolución depende de $\omega$. [cite_start]En el **Desafío 3**, se completó el círculo en aproximadamente 5.32 segundos[cite: 5].

---

### Observaciones Finales del Analista
* [cite_start]**Factor de Corrección:** Se determinó que el uso de un factor empírico de **1.04** es esencial para compensar el deslizamiento y las fricciones del simulador, asegurando la precisión en trayectorias complejas como el cuadrado[cite: 5].
* [cite_start]**Control en Lazo Abierto:** El método utilizado (temporización) es efectivo en Webots debido a que los parámetros físicos son exactos y reproducibles, aunque en un entorno real requeriría sensores externos (encoders) para corregir desviaciones[cite: 5].
---

## 6) Conclusión

Este laboratorio nos enseñó a trabajar los principios de la **cinemática de robots diferenciales** en un entorno controlado. Lo cual se logró:

* Implementar una máquina de estados simple basada en el tiempo para generar trayectorias geométricas.
* Comprender la relación entre la velocidad angular de los motores y el desplazamiento lineal del robot en el plano.
* Aprender a configurar entornos de simulación (arena, luces y sensores) para pruebas de robótica autónoma.

