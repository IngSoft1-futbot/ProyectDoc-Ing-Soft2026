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
