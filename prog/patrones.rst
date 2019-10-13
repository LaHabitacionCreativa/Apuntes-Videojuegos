===========================================================
Patrones de diseño clásicos
===========================================================

Creacionales
=====================================================
Construcción de clases, objetos y otras estructuras de datos.

Abstract factory, builder, factory method
------------------------------------------
Permiten crear diferentes instancias de objetos sin tener que preocuparnos de la forma en la que realmente se crean. Se usan cuando es necesario crear varios objetos y hay restricciones.

Ejemplo. Queremos crear PJs de distintas clases (arquero, caballero, mago) desde un único punto, y que al crearlos se cree también un equipamiento por defecto (arma, protección, etc) restringiendo que el arquero sea el único que pueda llevar arcos, etc.

- Abstract Factory. Se crean clases fábrica para cada tipo a crear, ofreciendo un punto desde el que crear lo que necesitemos. 

- Factory Method. Digamos que mete la funcionalidad en un método (redefinido en las subclases). Se utiliza para implementar el abstract factory.

- Builder. Separa el proceso de cómo se crean las intancias de su representación jerárquica. Digamos que añade control del proceso (orden) a una fabrica abstracta.


Prototype
------------------------------------------
El principal problema de los anteriores en la rigidez de la jerarquía de elementos, que aunque se puede cambiar añadir etc, es más costoso.
Este patrón permite clonar objetos en tiempo de ejecución.

Ejemplo. Catálogo de armas que puede crecer con nuevas, que varían dependiendo de alguna condición del juego, etc. Juego tipo 2 stick shooter en el que se generan distintos tipos de enemigos en puntos determinados podrían hacerle desde un enemigo "reina".


Singleton	
------------------------------------------
Una clase de la que sólo se permite que exista una instancia y proporcionar un punto de acceso global a ella. 

Ejemplo. Gestores de juego, música, bases de datos. Elementos únicos como balón de juego deportivo, bola de pinball, etc.

Object Pool
------------------------------------------
No pertenece a los patrones especificados por GoF. 
Cuando el cliente necesita un objeto, lo pide al pool, de modo que si existe se lo devuelve y si no se crea uno nuevo. Se puede considerar que el singleton es un caso particular de este patrón pero con 1 instancia en vez de n.
Con este patrón ahorramos memoria, ya que no creamos objetos innecesarios. Tampoco se destruyen innecesariamente, ya que al utilizarlos los devolvemos al pool.

Ejemplo. Ráfagas de disparos. Generación de escenarios "infinitos" compuestos por una secuencia de varias piezas posibles (carreras, infinite runners, etc.). 




Estructurales
========================
Forma de organizar las jerarquías de clases, las relaciones y las diferentes composiciones entre objetos para obtener un buen diseño en base a unos requisitos de entrada.


Adapter
------------------------------------------
Enlaza interfaces de diferentes clases. Partimos de tener dos interfaces que hemos de conectar, pero no podemos modificarlas (biblioteca externa, subsistema externo, etc) o sería costoso. Una solución sería crear unos nuevos elementos (adaptadores) encargados de adaptar las llamadas entre las dos.


Bridge  
------------------------------------------
Separa la interfaz de un objeto de su implementación.


Composite
------------------------------------------
Estructuras tipo árbol de objetos simples y compuestos.
Las operaciones están redefinidas y se propagan por el árbol: mover, atacar, calcular precio, etc.

Ejemplos. Juegos de estrategia, creación de grupos y comportamiento por tipo de unidad distinto para la misma operación (movimiento, ataque, etc.). 
Un juego en el que los jugadores pueden recoger objetos o items, los cuales tienen una serie de propiedades como precio, descripción, etc. Cada item, a su vez, puede contener otros items. Por ejemplo, un bolso de cuero puede contener una pequeña caja de madera que, a su vez, contiene un pequeño reloj dorado.


Facade  
------------------------------------------
Consiste en una clase que incluye sólo los elementos que nos interesan de un subsistema, ofreciendo un punto de acceso de mayor nivel de abstracción con el que comunicarse con ese subsistema.


Proxy	
------------------------------------------
En este patrón un objeto proxy (reducido) representa al real. El cliente accede al reducido y sólo se carga el real cuando se necesita. 

Ejemplo. Carga de partes de escenarios, imágenes, texturas, al aproximarnos o usarlos.



Decorator	
--------------------
Añade o modifica responsabilidades, funcionalidades o propiedades en tiempo de ejecución.

Ejemplo. Accesorios en armas, silenciador ruido, cargador extra, precisión, daño, alcance, etc.


Flyweight
------------------------------------------
Utilizado para ahorrar recursos. Separa los objetos en dos partes, una que nunca cambia (intrinseca) y otra dependiente del estado (extrinseca). 
De esta forma, aunque tengamos muchos objetos, almacenaremos todas sus instancias extrínsecas pero sólo necesitamos almacenar una instancia intrinseca por tipo de objeto. 

Ejemplo. Generación de un bosque en el que las instancias de árboles tienen muchas cosas en común (texturas, malla, etc.) y sólo cambian algunas (posición, color, tamaño).
Ejercito compuesto de varias clases de unidades (soldado, lancero, arquero), en el que hay atributos comunes (nivel, daño, etc.) y particulares (posición, vida, etc.).




Comportamiento
================
Envío de mensajes entre objetos y cómo organizar ejecuciones de diferentes métodos para conseguir realizar algún tipo de tarea de forma más conveniente.


Template Method
------------------------------------------
Separa un algoritmo complejo de forma que la parte común se implementa en la clase padre y la parte concreta en las hijas. 

Ejemplo. Los juegos de damas y ajedrez tienen una parte común (mover, esperar turno, etc.) y una parte concreta definida por las reglas de cada juego.



Iterator	
------------------------------------------
Acceso secuencial a elementos de una colección de datos, ocultando la representación interna.


Observer	
------------------------------------------
Notificar cambios a varias clases. Puede realizarse en C# con delegados y eventos.


Mediator	
------------------------------------------
En conjuntos de clases comunicadas entre sí de forma compleja, hace de pieza intermediaria para gestionar estas comunicaciones y hacer el sistema más simple y entendible.


Chain of Responsibility
------------------------------------------
Permite pasar peticiones entre una cadena de objetos, hasta que uno puede hacerse responsable de ella.


Command
------------------------------------------
Encapsula una orden como un objeto. Permite parametrizar clientes que responden en base a diferentes peticiones que le van llegando y ser capaz de ejecutar acciones (respuestas), deshacerlas y rehacerlas.

Ejemplo. Secuencia de movimientos que se pueden deshacer o rehacer. Mapeado de esquemas de botones.


Memento	
------------------------------------------
Captura y restaura el estado interno de un objeto.


Interpreter	
------------------------------------------
Permite crear un analizador sintáctico para un lenguaje propio.


State	
------------------------------------------
Altera el comportamiento del objeto cuando cambia de estado.

Ejemplo. Sirve para implementar autómatas (colecciones de estados y transiciones), como los estados de un personaje: quieto, saltando, corriendo, atacando, etc.


Strategy	
------------------------------------------
Encapsula una familia de algoritmos de forma que se puede intercambiar su uso sin modificar el cliente.

Ejemplo. Diferentes algoritmos de ataque. Diferentes comportamientos de la IA de los enemigos dependiendo del nivel de dificultad.


Visitor	
------------------------------------------
Permite realizar operaciones sobre una jerarquía de objetos, considerando que la operación puede realizarse de forma distinta dependiendo del tipo de objeto. 

Ejemplo. Recorrer una estructura para detectar colisisones y en cada caso realizar la acción necesaria dependiendo del objeto (si un misil choca con edificio, coche, etc.).




Enlaces
===============
Patrones de diseño orientados a videojuegos:

- `Game Programming Patterns <http://gameprogrammingpatterns.com/contents.html>`_
- `Implementación de varios patrones en Unity <https://github.com/Naphier/unity-design-patterns>`_
- `Delegados y eventos en Unity <http://www.theappguruz.com/blog/using-delegates-and-events-in-unity>`_
- Object Pooling en Unity `Unity3d.com <https://unity3d.com/es/learn/tutorials/modules/beginner/live-training-archive/object-pooling>`_ `Theappguruz <http://www.theappguruz.com/blog/object-pooling-in-unity-3d>`_ `Theappguruz2 <http://www.theappguruz.com/blog/infinite-track-generation-like-road-fighter-game>`_



Patrones de diseño clásicos:

- `Do Factory <http://www.dofactory.com/net/design-patterns>`_
- `Source Making <https://sourcemaking.com/design_patterns>`_
- E.Gamma, Patrones de Diseño. Addison-Wesley. 2002.

