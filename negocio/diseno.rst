=========================
Diseño
=========================


Documento de diseño
=========================

Modelos del libro level up 

- Documento de 1 pagina. fase inicial
- Documento de 10 paginas. fase de preproducción
- Documento de diseño. toda la informacion de diseño


Fases diseño iterativo
=========================

- Analisis
- Diseño
- Implementación
- Testeo


Tracy Fullerton
=========================

Elementos para entender qué es un juego

- los jugadores
- los objetivos
	- algo por lo que esforzarse, algo que lograr según las reglas, son retadores pero alcanzables. Ejemplos, capturar, perseguir, atrapar, construir, etc.
- los procedimientos
	- acciones que puede hacer el jugador para lograr los objetivos. Quien hace qué, cuándo y cómo.
- las reglas
	- objetos y acciones permitidas dentro del juego
- los recursos
	- bienes usados para alcanzar objetivos. Ejemplos, balas, dinero, vidas
- las fronteras
	- entrar en un mundo con sus reglas
- el conflicto
	- cuando los jugadores intentan alcanzar el objetivo usando las reglas y fronteras
- el resultado
	- es incierto para mantener la atención, por ejemplo no hace falta que sea cumplir el objetivo


Richard Bartle
=========================

Clasificación de jugadores

- Asesino (Killer)
- Socializador (Socializer)
- Explorador (Explorer)
- Cumplidor (Achiever)

Bartle´s player types

.. code-block:: c#

			   acting
		killers 20%	|	achievers 40%
	players -------------------------world, enviorenment
		socializers 80%	|	explorers 50%
			interacting


Marc Leblanc
=========================

Taxonomía de placeres

- Sensación (por los sentidos, oír, ver, etc)
- Fantasia (vivir en un mundo imaginario)
- Narrativa (más que por la historia, vivir momentos dramáticos)
- Reto (de lograr un objetivo)
- Comunidad (crear un grupo social)
- Descubrimiento (sorpresa de descubrir armas, objetos, escenarios, etc)
- Expresión (crear propios PJs, diferentes maneras de resolver)
- Sumisión (rendirse a las estructuras y límites del juego)

Game design para sensaciones,... `<http://www.8kindsoffun.com>`_


Reglas 
=========================

Instrucciones que el jugador debe conocer. Las va descubriendo. (Ej Monopoli, manual de instrucciones).

Mecánicas
=========================

Son reglas más detalladas. Se le ocultan al jugador ya que están implementadas en el sw (Ej Monopoli, reglas operacionales, economía, turnos, etc).

Una mecánica al principio del diseño estará poco detallada (ej. el pj muere al caer el lava) y luego se detalla más (ej. la lava sube 10px por seg, si toca al pj este pierde 10 puntos de vida por seg, etc).

Es importante que las mecánicas vayan apareciendo poco o poco y no estén disponibles desde el principio. Ejemplo, diablo 2 puedes recolectar objetos pero en el episodio 2 te dan el cubo orádrico para ver si puedes obtener algo mejor mezclando lo que tienes. Si lo hubieran dado de primeras no funcionaría.

Tipos (Game Mechanics: Advanced Game Design)

- Físicas
- Mecanismos de progresión (accesos a lugares bloqueados, ej palancas para activar, desbloquear)
- Maniobras tácticas (disponer unidades en mapa)
- Economía interna (objetos recolectados, intercambiados, vendidos (dinero, vida, municion, popularidad, etc.)).
- Interacción social


Flujo
=========================

Grado de estar centrado completamente en una actividad con un grado de nivel de disfrute y plenitud.
Para poner al jugador en estado de flujo.

- Objetivos claros. Que sepa qué tarea hacer.
- Evitar distracciones.
- Retroalimentaciones inmediatas.
- Retar continuamente (pero que no sea muy fácil (aburrimiento) o muy difícil (frustración)).

Mantener el estado de flujo. Es interesante hacer cambios de intensidad, con momentos que requieran más atención y otros más tranquilos (como en las películas).

Hay que hacerles sentir libertad pero en verdad no la tienen y la controlamos.
Ejemplo, elegir entre 2 puertas por donde ir, etc. Son opciones limitadas
Los guiamos para lo que queremos, mediante:

- Interfaz
- Diseño visual (puntos de referencia)
- Música


Niveles
=========================

Tipos

- Lineal (on rails, narración, tetris, mario)
- Semifinal. La más usada. Sensación de total libertad pero tiene cuellos de botella por los que hay que pasar (como conseguir unos puntos, pasar un tiempo, etc).
- No lineal. Sandbox. (minecraft).


Experiencia del jugador
=========================

1. Qué experiencia quiero ofrecer a mis jugadores? (sentirse en el espacio, adrenalina,…).
2. Cómo personalizo esa experiencia? (mi visión personal que quiero ofrecer)
3. Cuál es el público objetivo? (más que edad, sexo,… pensar en dispositivo, tiempo de juego, personas que les gusta hacer tal cosa, etc).


Juegos de plataformas
=========================

- Retos
    - Por entorno
        - mecanismos (activados al pulsar, empujar, etc)
        - trampas (huecos, pinchos, etc)
        - accesorios (objetos interactivos para romper, etc)
        - puzzles
    - Por enemigos
        - patrullero, perseguidor, disparador, bloqueador
- Progresion
    - Lineal (pasar niveles)




