## API de Comportamientos 

-Esta libreria proporciona las herramientas y la documentacion necesaria para que los usuarios puedan diseñar y programar la logica de comportamiento de sus jugadores utilizando el lenguaje Python. El sistema del juego procesara las decisiones tomadas en cada turno y ejecutara los comportamientos, resolviendo su exito y rendimiento fisico de forma automatica mediante los atributos del sistema PACSS (POWER, AGILITY, CONTROL , SPEED y STRENGTH) del jugador en cuestion.

## Punto de Entrada: El metodo jugar_turno

- Para que el sistema del simulador pueda comunicarse con el codigo del usuario, es obligatorio que el script de Python incluya una funcion principal llamada jugar_turno(estado_partida). Este metodo actua como el nucleo de acciones de los jugadores y es el unico lugar donde se podra desarrollar la logica de su comportamiento.

- Frecuencia De Ejecucion: El motor del juego no llama a esta funcion una sola vez. La invoca repetidamente varias veces por segundo durante cada "tick" o ciclo de actualizacion del partido para evaluar constantemente la situacion.

- Obtencion del Estado de la Partida: El metodo obtiene la informacion del entorno exclusivamente a traves de parametros. El motor del servidor inyecta el objeto estado_partida como argumento al momento de invocar jugar_turno. No se requieren llamadas a primitivas externas ni consultas de red por parte del usuario para recibir esta actualizacion, ya que la entrega de los datos es un proceso gestionado por el sistema.

- El Parametro De La Funcion Principal: Cada vez que el servidor llama a la funcion jugar_turno, inyecta un objeto llamado estado_partida. Este objeto actua como los sentidos del jugador y contiene informacion de solo lectura sobre el instante actual del partido, como:

    .Datos de la pelota: Sus coordenadas espaciales actuales y, en caso de que alguien la posea en ese instante, el ID de dicho jugador.
    
    .Datos de los jugadores: Coordenadas precisas tanto de compañeros como de rivales, las cuales seran utilizadas para calcular distancias y tomar decisiones de comportamiento.
    
    .Datos del partido: Informacion general sobre el entorno, como el tiempo restante y el marcador actual.
    
- Regla De Continuidad: Los jugadores solamente cambiaran su comportamiento actual si el script ejecuta explicitamente una nueva primitiva durante el turno. En caso de que el comportamiento finalice su accion y el sistema no reciba ningun cambio de primitiva, el mismo no detendra al bot, sino que lo mantendra realizando de forma ininterrumpida el comportamiento que ya tenia asignado en el tick anterior.


## ─── CATALOGO DE PRIMITIVAS ───

# moverse_hacia(x: float, y: float, porcentaje_velocidad: int) -> None

> Descripcion: Ordena al jugador trasladarse hacia las coordenadas (x, y) otorgadas como parametro, utilizando un porcentaje de velocidad coherente a su atributo speed.

> Impacto en el Estado: Actualiza progresivamente las coordenadas propias del jugador en los siguientes ticks.

> Atributos evaluados:

- SPEED: Determina la velocidad maxima de desplazamiento y la capacidad de aceleracion del jugador durante el trayecto.

# mantener_posicion(x: float, y: float, radio: float) -> None

> Descripcion: Conserva al jugador en una zona del campo especificada a traves del radio para establecer futuras estrategias defensivas o esperar un pase de un compañero. 

> Impacto en el Estado: Establece las coordenadas (x, y) ingresadas como el punto central de referencia y delimita el area utilizando el radio especificado. El bot se trasladara a esa zona y se mantendra en movimiento dentro de ese limite hasta que la pelota o un rival lo invadan.

> Atributos evaluados:

- SPEED: Utilizado por el sistema para reaccionar ante invasiones de rivales o de la pelota dentro de su radio establecido.

# marcar_pelota() -> None

> Descripcion: Ordena al jugador un intento de quite de la pelota hacia el jugador rival que posee el balon.Calcula dinamicamente las coordenadas actuales del compañero indicado por su ID y ejecuta un envio dirigido hacia su posicion para la circulacion tactica del equipo.

> Impacto en el Estado: Si la accion es exitosa, invierte la variable de posesion, asignando el balon al jugador actual 

> Atributos evaluados:

- CONTROL: El motor valida primero si el jugador esta lo suficientemente cerca del rival segun el radio de alcance dictado por este atributo.

- STRENGTH: El exito del robo se calcula mediante una disputa directa de este valor contra la fuerza del oponente.

# patear_pelota(x: float, y: float, porcentaje_fuerza: int) -> None

> Descripcion: El jugador ejecuta un golpe sobre la pelota con la direccion de las coordenadas (x, y) utilizando un porcentaje de la fuerza maxima del jugador.

> Impacto en el Estado: Impulsa la pelota con una velocidad y direccion determinadas, y aplica una restriccion temporal a la capacidad del jugador para volver a golpear.

> Atributos evaluados:

- CONTROL: El sistema verifica primero si la distancia entre el jugador y la pelota es menor o igual al radio de alcance dictado por este atributo.

- POWER: Escala el porcentaje de fuerza ingresado para calcular la velocidad de salida del pase.

- AGILITY: Determina la cantidad de ticks de penalizacion que sufrira el jugador antes de poder efectuar un nuevo disparo.

# pasar_pelota(x: jugador, porcentaje_fuerza: int) -> None

> Descripcion: Calcula las coordenadas actuales del compañero indicado por su ID y ejecuta un envio del balon dirigido hacia su posicion.

> Impacto en el Estado: Impulsa la pelota con una velocidad y direccion determinadas hacia la ubicacion del receptor al momento del pase y aplica una restriccion temporal a la capacidad del jugador para volver a golpear.

> Atributos evaluados:

- CONTROL: Verifica si la distancia entre el jugador y la pelota es valida para iniciar el pase.

- POWER: Escala el porcentaje de fuerza ingresado para calcular la velocidad de salida del pase.

- AGILITY: Determina la cantidad de ticks de penalizacion que sufrira el jugador antes de poder efectuar un nuevo disparo.



