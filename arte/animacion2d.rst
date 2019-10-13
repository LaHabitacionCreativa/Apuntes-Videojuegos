=========================
Workflow Animacion 2D
=========================


Arte
============

- Arte PJ completo (en digital o vectorial)
- Despiezar y retocar uniones
- Poner las piezas rectas (con transform) para que luego se creen bien las bounding boxes
- Exportar las capas como pngs independientes
- Meter en spriter,... y animar
- Exportar el atlas de cada animación
- Retocar detalles / errores a mano


Spriter
=========================

- Añadir a la carpeta del proyecto las imágenes sueltas
- Añadirlas al espacio de trabajo (ajustando tamaño y punto de pivotaje y ordenarlas en z)
- Poner huesos (con alt pulsado)
    - al ponerlos seleccionamos el anterior para que vaya haciendo la jerarquía
    - podemos comprobar los hijos seleccionando un hueso y pulsando z
- Unir huesos a sprites, seleccionar el hueso, pulsar b y pulsar el sprite
- Para mover
    - pulsamos cerca del hueso cuando sale el icono de rotación
    - si queremos hacerlo por cinemática inversa (IK), seleccionamos varios huesos y con el shift pulsado movemos con el icono de rotación
- Crear una posición base para copiarla siempre que empecemos una animación



Spriter2UnityDX

GitHub Link: `<https://github.com/Dharengo/Spriter2UnityDX>`_

Use Instructions:

1. Import the package into your Unity project (just drag and drop it into your Project view).
2. Import your entire Spriter project folder (including all the textures) into your Unity project
3. The converter should automatically create a prefab (with nested animations) and an AnimatorController
4. When you make any changes to the .scml file, the converter will attempt to update existing assets if they exist
5. If the update causes irregular behaviour, try deleting the original assets and then reimporting the .scml file



Dragonbones
=========================

- Crear proyecto
    - Añadir las imágenes a la subcarpeta librería (botón importar assets o copiarlas directamente)
- Colocar los elementos
    - Click en armature (por defecto está en stage)
    - Arrastrar los elementos a la escena (colocar, girar, escala, mirror)
    - Establecer el orden de dibujado en el eje z (pestaña draw order)
- Colocar los huesos
    - Botón create bone (o tecla N)
    - Si el anterior está seleccionado el nuevo lo crea como hijo
        - así que si estamos haciendo un brazo y vamos a hacer el otro, hay que deseleccionar (clickar en root)
    - Las imágenes las asocia automáticamente
        - si no lo hace, en la jerarquía arrastrar cada una al hueso y la pone como hija
- Animar
    - Configurar
        - para la primera imagen darle a la banderita add key frame (K)
        - pulsar banderita auto key para que cuando cambiemos algo lo guarde
        - pulsar botón papel cebolla
    - Vamos cambiando los elementos
        - si es una imagen podemos moverla (botón select o tecla V)
        - si es hueso (botón pose o tecla P)
            - si seleccionamos varios huesos hacemos IK
