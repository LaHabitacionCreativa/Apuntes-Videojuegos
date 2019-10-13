====================
Game Manager
====================

El objetivo es mantener en un único lugar todos los elementos comunes al estado del juego (puntos, nivel actual, etc.) y funcionalidad (guardar partida, etc.) para poder acceder a ellos desde cualquier parte (cualquier script).

Implementación sin el patrón Singleton
===============================================

- Crear una clase GameManager.cs con todo lo que necesito.
- Definir los atributos y métodos estáticos.
- Usar el método DontDestroyOnLoad para que la información no se destruya entre escenas.
- Crear un gameobject vacío y asociarle el script GameManager.cs.

Problemas

- No aseguro que haya un único game manager. Por ejemplo, si añado por error el game manager a otra escena, tendría 2, ya que el primero no se destruiría (DontDestroyOnLoad).
- Desde cada script que necesite el GameManager deberé:
    - Pasárselo (como objeto público)
    - O recuperarlo desde código (buscando el gameobject y obteniendo el componente).

Implementación con el patrón Singleton
================================================
Asegura que una clase tiene sólo una instancia y proporciona un único punto para acceder a ella.

Crear una clase GameManager.cs con todo lo que necesito.

- Definir los atributos y métodos públicos.
- Usar el método DontDestroyOnLoad para que la información no se destruya entre escenas.
- Constructor protegido (para no hacer new GameManager)
- Instancia estática de la clase
    - Al acceder a ella por primera vez, se creará.
    - Al acceder el resto de veces, devolverá la instancia creada.

No tengo que asignarlo a ninguna escena y puedo acceder desde cualquier lugar


.. code-block:: c#

    // Atributos
    GameManager.instancia.puntos++; 
    puntosvalue.text = GameManager.instancia.puntos.ToString();
    GameManager.instancia.estadoActual = TiposDeEstado.JUEGO;

    // Métodos
    GameManager.instancia.cambiarEscena("juego");
    GameManager.instancia.guardarPartida();



Patrón singleton
-------------------------------

.. code-block:: c#

    // ------------------------
    // Clase Game Manager
    // ------------------------
    public class GameManager : MonoBehaviour {

        // ------------------------
        // Atributos estaticos 
        // ------------------------

        //att privado (_instancia)
        static private GameManager _instancia;

        //att publico (instancia) por el que accedemos 
        static public GameManager instancia
        {
            // metodo get
            // se ejecuta al acceder por GameManager.instancia
            get
            {
                // si es la primera vez que accedemos a la instancia del GameManager, 
                // no existira, y la crearemos
                if (_instancia == null)
                {
                    // creamos un nuevo objeto llamado "_MiGameManager"
                    GameObject go = new GameObject("_MiGameManager");

                    // anadimos el script "GameManager" al objeto
                    go.AddComponent<GameManager>();

                    // guardamos en la instancia el objeto creado
                    // debemos guardar el componente ya que _instancia es del tipo GameManager
                    _instancia = go.GetComponent<GameManager>();

                    // hacemos que el objeto no se elimine al cambiar de escena
                    DontDestroyOnLoad(go);
                }

                // devolvemos la instancia
                // si no existia, en este punto ya la habra creado
                return _instancia;
            }

            // metodo set
            // no implementado para no permitir modificar la instancia "GameManager.instancia = x;"
        }

            // Constructor
            // Lo ocultamos el constructor para no poder crear nuevos objetos "sin control"
            protected GameManager(){}


Atributos
------------------------

.. code-block:: c#

    public int nivel;
    public int puntos;
    public int vida;

    // Control del estado
    public TiposDeEstado estadoActual;

    // Definimos previamente el enumerado
    // public enum TiposDeEstado { INTRO, MENU, JUEGO, SALIR }


Métodos
------------------

.. code-block:: c#

    using UnityEngine.SceneManagement;

        public void cambiarEscena(string escena) { 
            guardarPartida();
            SceneManager.LoadScene(escena); 
            }
        
        public void reiniciar(){
            guardarPartida();
            quitarPausa();
            SceneManager.LoadScene(SceneManager.GetActiveScene().name);
        }

        public void guardarPartida(){
            if(PlayerPrefs.GetInt ("nivelDesbloqueado") < nivel)
                PlayerPrefs.SetInt("nivelDesbloqueado", nivel);
            if(PlayerPrefs.GetInt ("puntosMax") < puntos) 
                PlayerPrefs.SetInt("puntosMax", puntos);
            }
        
        public void cargarPartida(){
            nivel = PlayerPrefs.GetInt("nivelDesbloqueado");
            puntos = PlayerPrefs.GetInt("puntosMax");
            }

        public void pausar(){
            // si esta activo lo pausamos
            if(Time.timeScale > 0){
                valorTimeScale = Time.timeScale;
                Time.timeScale = 0;
            }
            // sino, quitamos la pausa
            else{
                quitarPausa();
            }
            }
        public void quitarPausa(){
            if(Time.timeScale == 0) Time.timeScale = valorTimeScale;
            }


Enlaces
===================

Patrones de diseño

- `Game Programming Patterns <http://gameprogrammingpatterns.com/contents.html>`_
- `Do Factory <http://www.dofactory.com/net/design-patterns>`_
- `Source Making <https://sourcemaking.com/design_patterns>`_
- E.Gamma, Patrones de Diseño. Addison-Wesley. 2002.

Game Manager con patrón Singleton

- `Tutorial oficial Unity3d <https://unity3d.com/es/learn/tutorials/projects/2d-roguelike-tutorial/writing-game-manager?playlist=17150>`_
- `Artículo theappguruz <http://www.theappguruz.com/blog/managers-using-singleton>`_
- `Artículo packtpub <https://www.packtpub.com/books/content/creating-simple-gamemanager-using-unity3d>`_