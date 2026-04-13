## 6) Análisis de Preguntas

### 1. Código del Controlador en Webots
El siguiente código Python fue el efectivamente utilizado durante el laboratorio. Implementa los cuatro desafíos en un único controlador seleccionable mediante la variable DESAFIO. Se presentan los fragmentos relevantes para cada experimento analizado.
##### 1.1 Estructura general del controlador

```
from controller import Robot
import math 
robot = Robot() 
timestep = int(robot.getBasicTimeStep())
left_motor  = robot.getDevice('left wheel motor')
right_motor = robot.getDevice('right wheel motor')
left_motor.setPosition(float('inf'))
right_motor.setPosition(float('inf'))
left_motor.setVelocity(0.0)
right_motor.setVelocity(0.0)

Parámetros físicos del e-puck
base_speed             = 3.14    # rad/s
wheel_radius           = 0.0205  # m
distance_between_wheels = 0.052  # m (= L)

Parámetros del cuadrado (DESAFIO 4)
length_side    = 0.25
num_side       = 4
linear_velocity = wheel_radius * base_speed  # ≈ 0.0644 m/s
duration_side  = length_side / linear_velocity  # ≈ 3.88 s
angle_of_rotation = (2 * math.pi) / num_side  # π/2 rad
rate_of_rotation  = (2 * linear_velocity) / distance_between_wheels
duration_turn  = (angle_of_rotation / rate_of_rotation) * 1.04  # ≈ 0.66 s
```


### 2. Experimentos y Resultados
Se ejecutaron los cuatro desafíos del controlador. A continuación se presenta el código específico de cada uno, los cálculos cinemáticos derivados y los resultados observados en la simulación.

##### 2.1 DESAFIO 1 — Movimiento recto (vr = vl)
Código ejecutado:

```
elif DESAFIO == 1:  # recto
    vl, vr = base_speed, base_speed
    # → vl = 3.14,  vr = 3.14
```

Análisis cinemático:

```
v  = (vr + vl) / 2 = (3.14 + 3.14) / 2 = 3.14 rad/s
v_lineal = r × v   = 0.0205 × 3.14 ≈ 0.0644 m/s
ω  = r(vr - vl) / L = (3.14 - 3.14) / 0.052 = 0.0 rad/s
R  = ∞  (sin curvatura)
```

Resultados observados:
Al aplicar la misma velocidad a ambas ruedas (vl = vr = 3.14 rad/s), la velocidad angular ω resulta exactamente cero. El robot avanzó en línea recta sin ninguna desviación, manteniendo su orientación θ constante durante toda la simulación. La velocidad de traslación resultante fue de aproximadamente 0.0644 m/s en la dirección de la orientación inicial.
Este experimento valida la condición fundamental del modelo diferencial: cuando ambas ruedas giran a la misma velocidad, el punto de contacto de cada rueda con el suelo avanza la misma distancia en el mismo tiempo, impidiendo cualquier giro.

>Conclusión DESAFIO 1: vr = vl  →  ω = 0  →  θ = constante  →  trayectoria rectilínea.
>La rapidez de avance depende de la magnitud de la velocidad; la dirección no cambia.

##### 2.2 DESAFIO 2 — Trayectoria curva (vr ≠ vl)
Código ejecutado:
```
elif DESAFIO == 2:  # curva
    vl, vr = base_speed * 0.90, base_speed
    # → vl = 2.826,  vr = 3.14
```
Análisis cinemático:
```
vl = 3.14 × 0.90 = 2.826 rad/s
vr = 3.14 rad/s
v  = (3.14 + 2.826) / 2 = 2.983 rad/s  →  v_lineal ≈ 0.0611 m/s
ω = 0.0205 × (3.14 - 2.826) / 0.052 ≈ 0.124 rad/s
R  = L·(vr+vl) / (2·(vr-vl)) = 0.052 · 5.966 / (2 · 0.314) ≈ 0.494 m
```
Resultados observados:
Al reducir la velocidad de la rueda izquierda en un 10% respecto a la base (vl = 0.90 × base_speed), se generó una diferencia vr − vl = 0.314 rad/s que produjo una velocidad angular ω ≈ 0.124 rad/s. El robot describió un arco continuo hacia la izquierda con radio de curvatura R ≈ 0.494 m.
La rueda derecha (más rápida) quedó en el exterior del arco y la izquierda (más lenta) en el interior. La trayectoria fue un arco suave: pequeñas diferencias de velocidad producen grandes radios de curvatura. La orientación θ varió de manera continua y uniforme durante todo el recorrido.

>Conclusión DESAFIO 2: una diferencia del 10% en velocidades genera un giro continuo con R ≈ 0.49 m.
>Regla: mayor diferencia |vr − vl| → menor radio R → giro más cerrado.
>La rueda más lenta siempre queda en el interior del arco.

##### 2.3 DESAFIO 3 — Círculo (vl = 2.0, vr = 5.0)
Código ejecutado:
```
elif DESAFIO == 3:  # circulo
    vl, vr = 2.0, 5.0
```
Análisis cinemático:
```
v  = (5.0 + 2.0) / 2 = 3.5 rad/s   →   v_lineal = 0.0205 × 3.5 ≈ 0.0718 m/s
w = 0.0205 × (5.0 - 2.0) / 0.052 ≈ 1.18 rad/s
R  = 0.052 · 7.0 / (2 · 3.0) = 0.364 / 6.0 ≈ 0.0607 m
T_ciclo = 2π / ω = 2π / 1.18 ≈ 5.32 s  (tiempo por vuelta)
```
Resultados observados:
Con vl = 2.0 y vr = 5.0, la diferencia de velocidades (vr − vl = 3.0 rad/s) es sustancialmente mayor que en el DESAFIO 2, lo que produce un radio de curvatura muy pequeño (R ≈ 0.06 m). El robot describió un círculo cerrado de radio pequeño de forma continua y repetitiva. La trayectoria fue estable durante toda la simulación sin derivar.
Este experimento ilustra que un círculo perfecto se obtiene cuando la relación vr/vl = constante ≠ 1 se mantiene durante todo el tiempo. Con velocidades constantes en ambas ruedas, ω y v son constantes, por lo que el radio de curvatura es fijo y el robot traza el mismo círculo indefinidamente.

>Conclusión DESAFIO 3: vl = 2.0, vr = 5.0  →  R ≈ 0.061 m  →  círculo pequeño continuo.
>Para un círculo se requiere: vr ≠ vl, ambas del mismo signo, y ambas constantes en el tiempo.
>Cuanto mayor la diferencia vr − vl, más pequeño el círculo.

##### 2.4 DESAFIO 4 — Cuadrado (control por temporización)
Código ejecutado:
```
elif DESAFIO == 4:  # cuadrado
    cycle_time = duration_side + duration_turn
    phase = current_time % cycle_time
    if phase < duration_side:
        vl = base_speed   # avance recto
        vr = base_speed
    else:
        vl = -base_speed  # giro 90° en el lugar
        vr = base_speed
```
Parámetros calculados automáticamente por el controlador:
```
linear_velocity = 0.0205 × 3.14 ≈ 0.0644 m/s
duration_side   = 0.25 / 0.0644 ≈ 3.88 s  (tiempo para recorrer 0.25 m)
angle_per_side  = 2π / 4 = π/2 rad  (giro de 90°)
rate_of_rotation = 2 × 0.0644 / 0.052 ≈ 2.477 rad/s
duration_turn   = (π/2 / 2.477) × 1.04 ≈ 0.66 s  (con corrección 1.04)
cycle_time      = 3.88 + 0.66 = 4.54 s por lado
```
Análisis de las dos fases:
El cuadrado se construye alternando dos fases en cada iteración del ciclo principal:

| Fase | vl (rad/s) | vr (rad/s) | ω (rad/s) | Efecto |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| Avance | (3.88 s) | 3.14 | 0.0 | Recorre 0.25 m en línea recta |
| Giro  (0.66 s) | −3.14 | +3.14 | Gira 90° sobre su propio eje |

Resultados observados:
El robot ejecutó correctamente los cuatro lados del cuadrado. Durante la fase de avance (vl = vr = 3.14), el robot se desplazó en línea recta aproximadamente 0.25 m. Durante la fase de giro (vl = −3.14, vr = 3.14), el robot realizó una rotación pura de 90° sobre su eje, sin trasladarse.

El factor de corrección empírico 1.04 aplicado a duration_turn fue determinante: sin él, el giro quedaba 4% corto de los 90° requeridos, acumulando error angular en cada esquina del cuadrado. Con el factor, la figura resultante fue un cuadrado visualmente preciso de 0.25 m de lado.

Este experimento demuestra que trayectorias complejas pueden programarse sin sensores de posición mediante control por temporización, aunque la precisión depende de que no existan perturbaciones que desestabilicen el timing.

>Conclusión DESAFIO 4: el cuadrado se logra alternando dos estados: vl=vr (recto) y vl=-vr (giro puro).
>Duración lado: 3.88 s · Duración giro: 0.66 s · Ciclo total: 4.54 s
>El factor 1.04 compensa la inercia del robot y evita acumulación de error angular.

### 3. Tabla Resumen de Experimentos
La siguiente tabla sintetiza los resultados cuantitativos de los cuatro desafíos ejecutados, con los valores calculados a partir de los parámetros reales del robot e-puck.

| Desafío | Velocidades | v (m/s) | ω (rad/s) | Radio R (m) | Trayectoria observada |
| ------------ | ------------ | ------------ | ------------ | ------------ | ------------ |
| 1 — Recto | vl=vr=3.14 | 0.064 | 0.00 | ∞ | Línea recta continua |
| 2 — Curva | vl=2.83, vr=3.14 | 0.061 | 0.124 | 0.49 | Arco amplio hacia la izquierda |
| 3 — Círculo | vl=2.0, vr=5.0 | 0.072 | 1.18 | 0.061 | Círculo cerrado y repetitivo |
| 4 — Cuadrado | Temporización (2 fases) | Variable 0.064 | 2.48 | ∞ / 0 | Cuadrado de 0.25 m de lado |

### 4. Interpretación y Análisis
##### 4.1 La diferencia de velocidades controla la curvatura
El resultado más importante del laboratorio es que el comportamiento del robot diferencial está completamente gobernado por dos cantidades independientes: la suma de velocidades controla la rapidez de traslación, y la diferencia controla la curvatura. Matemáticamente:

`v ∝ (vr + vl)   →   a mayor suma, mayor velocidad lineal`  

`κ = 1/R ∝ (vr - vl)   →   a mayor diferencia, mayor curvatura`  

Esta separación casi ortogonal entre traslación y rotación hace que el modelo diferencial sea especialmente sencillo de programar. En el DESAFIO 2 bastó cambiar un 10% en una rueda para generar un giro continuo. En el DESAFIO 3, una diferencia de 3.0 rad/s produjo un círculo muy cerrado. En el DESAFIO 4, la alternancia entre diferencia cero (avance) y diferencia máxima con signos opuestos (giro puro) construyó el cuadrado.
##### 4.2 No-holonomía y sus implicaciones
El robot diferencial es un sistema no holónomo: no puede desplazarse lateralmente. Solo puede moverse en la dirección en que apunta. Esta restricción implica que el estado (x, y, θ) evoluciona de forma acoplada: cambiar θ requiere consumir tiempo y espacio. En el DESAFIO 4 esto es especialmente evidente: para dibujar cada esquina del cuadrado el robot debe detenerse (v = 0) y girar en el lugar antes de continuar. No puede simplemente cambiar de dirección.
##### 4.3 Control por temporización vs. control por sensores
Los desafíos 1 a 3 son ejemplos de control en lazo abierto: el robot ejecuta velocidades fijas sin retroalimentación. El desafío 4 agrega una capa de lógica temporal (duration_side, duration_turn) para construir una trayectoria compuesta. En condiciones de simulación ideal (sin fricción variable, sin ruido) este enfoque funciona correctamente.
Sin embargo, en un robot físico, el control por temporización acumula error: pequeñas variaciones en la fricción, el voltaje de las baterías o la superficie hacen que duration_side y duration_turn sean inexactos, produciendo un cuadrado cada vez más distorsionado. El factor 1.04 del código es precisamente una corrección empírica de este tipo de error sistemático. En robótica real se usan encoders (odometría) o sensores externos para cerrar el lazo y corregir la trayectoria.

### 5. Preguntas de Análisis
##### Pregunta 1: ¿Qué ocurre cuando ambas ruedas tienen la misma velocidad?
Cuando vr = vl, la diferencia (vr − vl) = 0, por lo tanto:

`ω = r·(vr - vl) / L = 0 / L = 0  rad/s`  

`v = (vr + vl) / 2 = vr  (= vl)`  

`R = ∞  (radio de curvatura infinito)`  

La velocidad angular es exactamente cero, lo que significa que la orientación θ no cambia. El robot avanza en línea recta en la dirección de su orientación actual con velocidad lineal igual a la velocidad individual de cada rueda. Geométricamente esto corresponde a un radio de curvatura infinito, es decir, una recta.
En el DESAFIO 1 esto fue confirmado experimentalmente: con vl = vr = 3.14, el robot avanzó en perfecta línea recta sin ninguna deriva angular durante toda la duración de la simulación.

>Respuesta: vr = vl  →  ω = 0  →  el robot avanza en línea recta.
>La velocidad de traslación es v = vr = vl. La orientación no varía.

##### Pregunta 2: ¿Cómo cambia la trayectoria cuando las velocidades son diferentes?
Cuando vr ≠ vl (ambas del mismo signo), la diferencia genera una velocidad angular ω ≠ 0 y el robot describe un arco circular de radio finito. La relación es:

`ω = r · (vr - vl) / L`  

`R = L · (vr + vl) / (2 · (vr - vl))`  

La dirección del giro depende del signo de (vr − vl): si vr > vl, el robot gira hacia la izquierda (la rueda más lenta queda en el interior del arco). Si vl > vr, gira a la derecha. El radio R es inversamente proporcional a la diferencia: mayor diferencia → menor radio → giro más cerrado.
En el DESAFIO 2 se demostró con una diferencia del 10%: vl = 2.826, vr = 3.14 produjo ω ≈ 0.124 rad/s y un arco con R ≈ 0.494 m. En el DESAFIO 3, una diferencia de 3.0 rad/s produjo ω ≈ 1.18 rad/s y un círculo con R ≈ 0.061 m, mucho más cerrado.

>Respuesta: vr ≠ vl  →  ω ≠ 0  →  arco circular de radio R = L(vr+vl)/(2(vr-vl)).
>Mayor diferencia |vr−vl| → menor R → curva más cerrada.
>La rueda más lenta queda siempre en el interior del arco.

##### Pregunta 3: ¿Qué ocurre cuando una rueda gira en sentido opuesto a la otra?
Cuando vr = −vl (velocidades de igual magnitud y signos opuestos):

`v = (vr + vl) / 2 = (vr - vr) / 2 = 0   →  sin traslación`  

`ω = r · (vr - vl) / L = r · (vr - (-vr)) / L = 2 · r · vr / L`  

`R = 0   →  el centro de rotación está en el punto medio entre las ruedas`  

La velocidad lineal del centro del robot es exactamente cero: el robot no se desplaza. Sin embargo, gira sobre su propio eje a una velocidad angular ω = 2·r·vr/L. Este es el movimiento de rotación pura, y es el más eficiente para cambiar de orientación sin consumir espacio.

En el DESAFIO 4 esta rotación pura se aplica en cada esquina del cuadrado: con vl = −3.14 y vr = 3.14, el robot gira en el lugar a ω = 2 × 0.0205 x 3.14 / 0.052 ≈ 2.48 rad/s, ejecutando exactamente 90° en cada giro gracias al control por temporización con el factor corrector 1.04.

>Respuesta: vr = −vl  →  v = 0, ω = 2·r·vr/L  →  rotación pura sobre el eje del robot.
>El robot no avanza. Gira sobre el punto medio entre sus ruedas.
>En el DESAFIO 4 se usa esta rotación para las esquinas del cuadrado.

##### Pregunta 4: ¿Qué tipo de movimiento permite dibujar un círculo?
Para dibujar un círculo se requiere que el robot mantenga una relación de velocidades constante vr/vl = constante, con vr ≠ vl y ambas del mismo signo positivo. Esto garantiza que ω y v sean constantes en el tiempo, produciendo un radio de curvatura fijo R.

`Condición: vr = cte,  vl = cte,  vr ≠ vl,  mismo signo`  

`→ ω = constante,  v = constante,  R = L(vr+vl)/(2(vr-vl)) = constante`  

`→ trayectoria circular perfecta`  


El robot cierra el círculo cuando θ acumula exactamente 2π radianes. El tiempo para completar un giro completo es:

`T = 2π / ω = 2π / r · ((vr - vl) / L)`  

En el DESAFIO 3 se verificó esto con vl = 2.0 y vr = 5.0:

`ω = 0.0205 · (5.0 - 2.0) / 0.052 = 1.18 rad/s`  

`R = 0.052 × (5.0 + 2.0) / (2 × 3.0) = 0.0607 m`  

`T = 2π / 1.18 ≈ 5.32 s por vuelta`  


El resultado experimental confirmó un círculo cerrado y repetitivo, con la rueda derecha (vr = 5.0) en el exterior del arco y la izquierda (vl = 2.0) en el interior.

>Respuesta: para un círculo se necesita vr ≠ vl, constantes, mismo signo.
>En DESAFIO 3: vl=2.0, vr=5.0 → R ≈ 0.061 m, T ≈ 5.32 s por vuelta.
>A diferencia de la rotación pura (R=0), en el círculo el robot también se traslada (v ≠ 0).

## 7) Conclusiones
Los cuatro desafíos ejecutados en Webots permitieron verificar experimentalmente los tres comportamientos fundamentales del modelo cinemático diferencial y construir una trayectoria compuesta (cuadrado) mediante control por temporización. Las conclusiones principales son:

* El modelo cinemático diferencial es simple y poderoso: dos velocidades escalares determinan completamente el movimiento plano del robot. La suma controla la rapidez y la diferencia controla la curvatura.
* DESAFIO 1 (vr = vl): confirmó la condición de movimiento recto. La igualdad de velocidades anula la velocidad angular y produce una trayectoria rectilínea perfecta con θ constante.
* DESAFIO 2 (vr ≠ vl, 10% de diferencia): demostró que pequeñas diferencias de velocidad producen giros continuos con radio amplio (R ≈ 0.49 m). La curvatura es proporcional a la diferencia.
* DESAFIO 3 (vl=2.0, vr=5.0): generó un círculo cerrado de radio pequeño (R ≈ 0.06 m). Para un círculo perfecto las velocidades deben ser constantes en el tiempo.
* DESAFIO 4 (cuadrado): demostró que trayectorias complejas pueden construirse combinando estados simples. La rotación pura (vl = −vr) permite girar 90° en el lugar sin trasladarse. El factor de corrección empírico 1.04 fue esencial para la precisión de los giros.
* El control por temporización (lazo abierto) funciona en simulación, pero en robots físicos acumula error. Para aplicaciones reales se requieren encoders o sensores externos que permitan cerrar el lazo de control.
* La plataforma Webots con el robot e-puck es apropiada para estudiar cinemática diferencial: los parámetros físicos del modelo son exactos y reproducibles, eliminando el ruido sensorial presente en los robots reales.
* Comprender la relación entre la velocidad angular de los motores y el desplazamiento lineal del robot en el plano.
* Aprender a configurar entornos de simulación (arena, luces y sensores) para pruebas de robótica autónoma.

