# DIAGRAMA DE CLASES
```mermaid
classDiagram
    class Usuario {
        +int id
        +String nickname
        +String email
        +String password
        +String avatar

        +registrarse()
        +iniciarSesion()
        +gestionarPerfil()
    }

    class Equipo {
        +int id
        +String nombre
        +List~Jugador~ jugadoresTitulares
        +List~Jugador~ jugadoresSuplentes
        
        +crearEquipo()
        +aptoLiga()
        +unirseALiga()
        +editarEquipo()
        +eliminarEquipo()
        +asignarComportamiento()
    }
    
    class Historial{
        +int ligasGanadas
        +int puntosTotales
        +int partidosGanados
        +int partidosPerdidos
        +int partidosEmpatados
        +List~Partido~ partidos
    }

    class Jugador {
        +int id
        +int dorsal
        +String nombre
        +int power
        +int agility
        +int control
        +int speed
        +int strength

        +actualizarJugador()
        +borrarJugador()
        +validarPACSS()
    }

    class Comportamiento {
        +int id
        +String nombre
        +String codigoPython

        +crearComportamiento()
        +validarSintaxis()
        +validarSeguridad()
        +editarComportamiento()
        +eliminarComportamiento()
        +comportamientoEnUso()
    }

    class Liga {
        +int id
        +String nombre
        +bool esPrivada
        +String contraseña
        +int minEquipos
        +int maxEquipos
        +int duracionPartido
        +int fechaInicio
        +List~Equipo~ inscritos

        +crearLiga()
        +iniciarLiga()
        +cancelarLiga()
    }

    class SolicitudAmistoso {
        +int id
        +String estado
        
        +aceptar()
        +rechazar()
    }

    class TablaPosiciones {
        +List~Equipo~ inscritos
        +String orden

        +actualizar()
        +resetear()
    }

    class RankingGlobal {
        +List~Usuario~ usuarios
        +String orden

        +actualizarRanking()
    }

    class Partido {
        +int id
        +bool enCurso 
        +Resultado resultadoPart

        +iniciarPartido()
        +procesarSustitucion()
        +finalizarPartido()
    }

    class Resultado {
        +int golesEquipo1
        +int golesEquipo2

        +quienGano()
        +esEmpate()
    }

    Usuario "1" -- "1" Equipo : posee
    Usuario "1" --> "*" Comportamiento : crea
    Usuario "*" --> "1" Partido: observa
    Usuario "1" --> "*" Jugador: crea
    
    Usuario "1" --> "*" SolicitudAmistoso: envia/recibe
    SolicitudAmistoso "1" --> "1" Partido: genera

    Equipo "1" o-- "6" Jugador : conforma
    Equipo --> Historial
    Equipo "*" --> "1" Comportamiento : asigna a jugador

    Liga "1" o-- "*" Equipo : inscribe
    Liga "1" *-- "1" TablaPosiciones : gestiona
    Liga "1" *-- "*" Partido : programa

    Partido "*" --> "2" Equipo : disputan

    Partido --> Resultado: tendra

    Usuario "*" --> "1" RankingGlobal: compone
```
# Diccionario de clases

### 1. Entidades Principales

| Entidad | Campo | Tipo de Dato | Descripción |
| :--- | :--- | :--- | :--- |
| **Usuario** | `user_id` | Integer | Identificador único autogenerado. |
| **Usuario** | `username` , `email` , `password` | String | Credenciales de acceso únicas. |
| **Usuario** | `created_at` | Datetime | Fecha y hora de registro. |
| **Jugador** | `player_id` , `team_id` , `shirt_numb` | Integer | IDs de relación y dorsal. |
| **Jugador** | `pacss_attributes` | Object | Valores numéricos enteros de *power*, *agility*, *control*, *speed* y *strength*. |
| **Equipo** | `jugadores_titulares` / `suplentes` | Array | Lista de objetos vinculando `player_id` y `behavior_id`. |

---

### 2. Entidades de Competición

| Entidad | Campo | Tipo de Dato | Descripción |
| :--- | :--- | :--- | :--- |
| **Liga** | `is_private` | Boolean | Define si requiere contraseña de acceso. |
| **Liga** | `min_teams` , `max_teams` , `duration` | Integer | Reglas numéricas y cupos. |
| **Partido** | `home_team_id` , `away_team_id` | Integer | Equipos contrincantes. |
| **Partido** | `scheduled_at` | Datetime | Fecha programada para el inicio automático. |
| **Amistoso** | `status` | String | Estado de la solicitud (ej: `"pedniente"`, `"aceptada"`, `"rechazada"`). |

---

### 3. Entidades de Comportamiento

| Entidad | Campo | Tipo de Dato | Descripción |
| :--- | :--- | :--- | :--- |
| **Behavior** | `behavior_id` | Integer | Identificador de la táctica. |
| **Behavior** | `name` | String | Nombre descriptivo asignado por el usuario. |
| **Behavior** | `code` | String | Bloque de texto que almacena el script en Python. |
