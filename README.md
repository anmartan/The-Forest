# IAVFinal-MartinSanchez
 
 Esta es la última práctica de la asignatura de Inteligencia Artificial para Videojuegos de la Universidad Complutense de Madrid.
 En ella, se ha desarrollado un prototipo de IA que resuelva un laberinto, sin saber a priori dónde están las salidas.

# Drive donde se encuentran los vídeos

Como parte de la documentación, se pueden ver los vídeos publicados en este [link](https://drive.google.com/drive/folders/1Sdmk3rVflRYJY84pAECgjzoPlQ-2-ANC?usp=sharing), para observar las pruebas que se han realizado y los resultados obtenidos.

Los vídeos se grabaron desde el editor de Unity, pero estas mismas pruebas se pueden replicar con el ejecutable.

# Descripción de la práctica

El famoso cuento infantil de Caperucita Roja nos sirve de inspiración para estudiar cómo se desenvuelve nuestra Inteligencia Artificial a la hora de resolver laberintos. Caperucita va a llevar a una cesta de comida para su abuela que, una vez más, ha vuelto a enfermar. 

Para llegar a casa de su abuela, debe atravesar un bosque repleto de árboles y por el que es fácil perderse. Por eso, Caperucita solo andará por los caminos que ya hay marcados en el suelo, sabiendo que, al menos uno, le llevará a su destino. 

El prototipo que se va a desarrollar, por tanto, es un laberinto en el que Caperucita se moverá, siguiendo una estrategia determinada, para intentar llegar a la salida del laberinto. 

El jugador puede controlar a Caperucita e intentar resolver el laberinto por su cuenta, o desactivar el movimiento del jugador y activar la IA, que hará el mismo trabajo. La IA tendrá varios niveles de “inteligencia”, entre los que el jugador puede escoger. En cada uno de estos niveles, el algoritmo empleado es más sofisticado.

Es importante recalcar que la IA no sabe dónde está la salida del laberinto, por lo que no puede utilizar algoritmos como A* o Dijkstra para encontrar el camino más corto. Esto supone que no siempre encontrará el camino óptimo, o que no todos los algoritmos se adaptan a todos los laberintos (con algunos tipos de laberintos, hay algoritmos que no pueden encontrar la solución).

# Descripción de la escena

La simulación empieza en un menú, con el que el jugador puede escoger qué mapa se va a utilizar, y qué algoritmo utilizará Caperucita (si el jugador quiere intentar resolver el laberinto por su cuenta, también puede).

Cuando el jugador decide empezar la simulación, encontrará un escenario un laberinto y a Caperucita en la entrada. Si ha escogido que sea la IA quien resuelva el laberinto, Caperucita empezará a moverse automáticamente. Además, a su paso, dejará un rastro que permite ver el camino recorrido hasta ahora.

# Descripción de la IA

En este caso, la IA se puede mover siguiendo cuatro comportamientos diferentes. Estos algoritmos son los más utilizados a la hora de resolver laberintos, y cada uno tiene unas ventajas y desventajas. Los algoritmos empleados son:
    
## Resolutor aleatorio. 
En cada intersección, decide qué dirección tomar, aleatoriamente.
- Ventajas: puede resolver, teóricamente, cualquier tipo de laberinto que tenga solución, ya que no tiene ningún tipo de limitación en el movimiento. 
- Desventajas: puede repetir el camino que ya ha recorrido, repetir infinitamente un camino, o no encontrar la salida nunca. No tiene memoria.

## Regla de la mano (izquierda o derecha). 
![Ejemplo explicativo de la regla de la mano izquierda](https://user-images.githubusercontent.com/48750779/122298300-dd9bfc80-cefc-11eb-8e3a-fbe209d4c56a.gif)

El personaje "pega la mano a una pared" (izquierda o derecha, según se le indique) y la sigue. 
- Ventajas: si el laberinto es simple (todas las paredes están interconectadas), encuentra siempre una solución, sin repetir caminos.
- Desventajas: si hay diferentes grupos de paredes, que no están conectados entre sí, puede no encontrar una solución. Si sigue un pilar, no podrá dejar de seguirlo (y no encontrará la solución). No tiene memoria.
## Algoritmo de Tremaux. 

![Ejemplo explicativo del algoritmo de Tremaux, de Wikipedia](https://user-images.githubusercontent.com/48750779/122298028-89911800-cefc-11eb-8285-5f1d855a2724.gif)

En cada intersección, decide qué dirección tomar, siguiendo cuatro normas sencillas:

        - Cuando llegas a una intersección (y cuando sales de ella), marcas el pasillo por el que has llegado (o por el que sales).
        - Nunca salgas de una intersección por un pasillo que tenga dos marcas.
        - Si al llegar a una intersección, encuentras una marca, vuelve atrás.
        - Si al intentar volver atrás no puedes (porque el pasillo tiene dos marcas), ve por el pasillo con menos marcas.
 - Ventajas: es una mejora importante respecto al resolutor aleatorio. Tiene memoria; cuando marca un pasillo por segunda vez, no vuelve a cruzarlo. Es capaz de detectar laberintos que no tienen solución. Si el laberinto tiene solución, siempre la encuentra.
 - Desventajas: siempre que encuentra un camino con una marca, deshace sus pasos. Esto puede hacer tarde mucho más tiempo en encontrar la solución, si la hay.
 
## Algoritmo de "Pledge". 
![image](https://user-images.githubusercontent.com/48750779/122297895-5b133d00-cefc-11eb-817a-48b7fa867192.png)

Intenta avanzar en una dirección "favorita", escogida arbitrariamente. Lleva un contador de giros, y se rige por estas normas:

        - Siempre te mueves en tu dirección favorita, hasta que te encuentras una pared que te lo impide.
        - En ese momento, empieza a seguir la pared (izquierda o derecha), y pon tu contador de giros a 0. 
        - Si sigues la pared izquierda, cada vez que haces un giro en sentido contrario a las agujas del reloj, suma uno a tu contador de giros; si giras en sentido horario, resta uno.
        - Si sigues la pared derecha, cada vez que haces un giro en sentido contrario a las agujas de reloj, resta uno a tu contador de giros; si giras en sentido horario, suma uno.
        - Si tu contador de giros está a 0 (no vale un múltiplo de 4) y estás mirando en tu dirección favorita, deja de seguir la pared.
- Ventajas: es una mejora importante respecto a la regla de la mano derecha. Si la salida está en una pared desconectada de la entrada, y en un "anillo exterior", este algoritmo puede encontrar la salida. 
- Desventajas: no puede encontrar la salida si la situación es la contraria: si el jugador empieza en un "anillo" exterior al de la salida. No tiene memoria.

# Pruebas realizadas y resultados obtenidos

Como pruebas, se han utilizado todos los algoritmos en cada mapa. Aquí se muestran los resultados:

## Mapa Braid (1): 298 casillas navegables

- Resolutor aleatorio
  - Casillas recorridas: 1684
  - Cobertura del mapa: 100% (298 casillas diferentes)

- Regla de la mano izquierda
  - Casillas recorridas: 168
  - Cobertura del mapa: 45.97% (137 casillas diferentes)

- Regla de la mano derecha
  -  Casillas recorridas: 156
  -  Cobertura del mapa: 50.33% (150 casillas diferentes)

- Algoritmo de Tremaux
  -  Casillas recorridas: 108
  -  Cobertura del mapa: 29.53% (88 casillas diferentes)

- Algoritmo de pledge (mano izquierda)
  - Casillas recorridas: 168
  - Cobertura del mapa: 45.63% (136 casillas diferentes)

- Algoritmo de pledge (mano derecha)
  -  Casillas recorridas: 132
  -  Cobertura del mapa: 44.29% (132 casillas diferentes)

- Media de casillas recorridas: 146.4 (ignorando el resultado obtenido por el resolutor aleatorio, por ser un valor atípico).
- Cobertura media: 43.15%, con todos los valores por debajo del 50.5% (ignorando el resultado obtenido por el resolutor aleatorio, por ser un valor atípico).

## Mapa Braid (2): 296 casillas navegables

- Resolutor aleatorio
  - Casillas recorridas: 1116
  - Cobertura del mapa: 73.31% (217 casillas diferentes)

- Regla de la mano izquierda
  - Casillas recorridas: 128
  - Cobertura del mapa: 32.09% (95 casillas diferentes)

- Regla de la mano derecha
  -  Casillas recorridas: 172
  -  Cobertura del mapa: 49.66% (147 casillas diferentes) 

- Algoritmo de Tremaux
  -  Casillas recorridas: 108
  -  Cobertura del mapa: 29.53% (88 casillas diferentes)

- Algoritmo de pledge (mano izquierda)
  - Casillas recorridas: 208
  - Cobertura del mapa: 52.03% (153 casillas diferentes)

- Algoritmo de pledge (mano derecha)
  -  Casillas recorridas: 172
  -  Cobertura del mapa: 49.66% (147 casillas diferentes)

- Media de casillas recorridas: 157.6 (ignorando el resultado obtenido por el resolutor aleatorio, por ser un valor atípico).
- Cobertura media: 42.59%, con todos los valores por debajo del 52.5% (ignorando el resultado obtenido por el resolutor aleatorio, por ser un valor atípico).

## Mapa Perfecto (1): 287 casillas navegables

- Resolutor aleatorio
  - Casillas recorridas: 240
  - Cobertura del mapa: 48.78% (140 casillas diferentes)

- Regla de la mano izquierda
  - Casillas recorridas: 332
  - Cobertura del mapa: 74.21% (213 casillas diferentes)

- Regla de la mano derecha
  -  Casillas recorridas: 244
  -  Cobertura del mapa: 60.97% (175 casillas diferentes)

- Algoritmo de Tremaux
  -  Casillas recorridas: 308
  -  Cobertura del mapa: 65.91% (203 casillas diferentes)

- Algoritmo de pledge (mano izquierda)
  - Casillas recorridas: 348
  - Cobertura del mapa: 76.31% (219 casillas diferentes)

- Algoritmo de pledge (mano derecha)
  -  Casillas recorridas: 244
  -  Cobertura del mapa: 73.51% (211 casillas diferentes)

- Media de casillas recorridas: 286
- Cobertura media: 66.62%
- 
## Mapa Perfecto (2): 287 casillas navegables

- Resolutor aleatorio
  - Casillas recorridas: 276
  - Cobertura del mapa: 56.79% (163 casillas diferentes)

- Regla de la mano izquierda
  - Casillas recorridas: 380
  - Cobertura del mapa: 81.53% (234 casillas diferentes)

- Regla de la mano derecha
  -  Casillas recorridas: 256
  -  Cobertura del mapa: 71.43% (205 casillas diferentes)

- Algoritmo de Tremaux
  -  Casillas recorridas: 308
  -  Cobertura del mapa: 79.44% (228 casillas diferentes)

- Algoritmo de pledge (mano izquierda)
  - Casillas recorridas: 380
  - Cobertura del mapa: 81.53% (234 casillas diferentes)

- Algoritmo de pledge (mano derecha)
  -  Casillas recorridas: 256
  -  Cobertura del mapa: 34.84% (100 casillas diferentes)

- Media de casillas recorridas:  (ignorando el resultado obtenido por el resolutor aleatorio).
- Cobertura media: %, con todos los valores por debajo del %.

## Mapa sin salida
En este caso, no se han recogido datos, puesto que todos los algoritmos quedan buscando la salida indefinidamente. Solamente el algoritmo de Tremaux es capaz de volver a la entrada (y concluir que el laberinto no tiene solución).
![Algoritmo de Tremaux en un laberinto sin solución](https://user-images.githubusercontent.com/48750779/122249225-f2f73380-cec8-11eb-97dc-3bf7fe3a23b8.png)

## Conclusiones a partir de los resultados
De acuerdo a los datos recogidos, parece evidente que el algoritmo que peores resultados da es el resolutor aleatorio. El resto de algoritmos, aunque con peculiaridades, dan resultados similares.

Cabe destacar también que, en laberintos perfectos, encontrar la salida es más costoso para todos los algoritmos, ya que acaban con mayor frecuencia en puntos muertos. Al contrario que los demás, el resolutor aleatorio mejora su eficiencia, ya que no hay tantas intersecciones con varias salidas disponibles.

# Bibliografía y recursos utilizados

- Unity 2018 Artificial Intelligence Cookbook, Second Edition [(Repositorio)]( https://github.com/PacktPublishing/Unity-2018-Artificial-Intelligence-Cookbook-Second-Edition)
- Unity Artificial Intelligence Programming, Fourth Edition [(Repositorio)]( https://github.com/PacktPublishing/Unity-Artificial-Intelligence-Programming-Fourth-Edition)
- Mazes For Programmers: Code Your Own Twisty Little Passages, First Edition [(Página del libro)](http://www.mazesforprogrammers.com/)
- [Think Labyrinth](http://www.astrolog.org/labyrnth/algrithm.htm), con una lista de algoritmos útiles para resolver laberintos, desde distintas perspectivas y con diferentes restricciones.

