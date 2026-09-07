## API de Comportamientos 

-Esta libreria proporciona las herramientas y la documentacion necesaria para que los usuarios puedan diseñar y programar la logica de comportamiento de sus jugadores utilizando el lenguaje Python. El sistema del juego procesara las decisiones tomadas en cada turno y ejecutara los comportamientos, resolviendo su exito y rendimiento fisico de forma automatica mediante los atributos del sistema PACSS (Posicionamiento, Ataque, Control, Velocidad y Fuerza) del jugador en cuestion.

## Punto de Entrada: El metodo jugar_turno

-Para que el sistema del simulador pueda comunicarse con el codigo del usuario, es obligatorio que el script de Python incluya una funcion principal llamada jugar_turno(estado_partida). Este metodo actua como el cerebro del bot y es el unico lugar donde se podra desarrollar la logica de su comportamiento.
> Frecuencia De Ejecucion: El motor del juego no llama a esta funcion una sola vez. La invoca repetidamente varias veces por segundo durante cada "tick" o ciclo de actualizacion del partido para evaluar constantemente la situacion.
> El Parametro De La Funcion Principal: Cada vez que el servidor llama a la funcion jugar_turno, inyecta un objeto llamado estado_partida. Este objeto actua como los sentidos del jugador y contiene informacion de solo lectura sobre el instante actual del partido, como:
    -Datos de la pelota: Sus coordenadas espaciales actuales y, en caso de que alguien la posea en ese instante, el ID de dicho jugador.
    -Datos de los jugadores: Coordenadas precisas tanto de compañeros como de rivales, las cuales seran utilizadas para calcular distancias y tomar decisiones de comportamiento.
    -Datos del partido: Informacion general sobre el entorno, como el tiempo restante y el marcador actual.
> Regla De Continuidad: Los jugadores solamente cambiaran su comportamiento actual si el script ejecuta explicitamente una nueva primitiva durante el turno. En caso de que el comportamiento finalice su accion y el sistema no reciba ningun cambio de primitiva, el mismo no detendra al bot, sino que lo mantendra realizando de forma ininterrumpida el comportamiento que ya tenia asignado en el tick anterior.

# ─── PRIMITIVAS ───

> Moverse_Hacia(): 
> Mantener_posicion():
> Marcar_pelota(): 
> Patear_pelota():
> Pasar_pelota():
> Proteger_pelota():


