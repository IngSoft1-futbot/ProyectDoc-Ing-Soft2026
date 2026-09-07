### Caso de uso: Listar ligas de un usuario

#### Descripción
Permite a un usuario consultar la lista de todas las ligas que ha creado o en las que se encuentra participando actualmente.

#### Atributos
* **Actor Primario:** Usuario
* **Actor Secundario:** Sistema
* **Precondición:** 
  1. El Usuario debe estar autenticado en la plataforma.

#### Inputs
1. ID del Usuario o consulta de selección ("Mis Ligas").

#### Escenario Exitoso
1. El Usuario navega a la sección "Mis Ligas" o consulta su perfil.
2. El Sistema solicita a la base de datos la lista de ligas asociadas al ID del Usuario (ya sea como creador o como participante con su equipo).
3. El Sistema obtiene los registros y los procesa.
4. El Sistema muestra en pantalla el listado de ligas incluyendo información básica (Nombre, ID de la liga, Estado, N° de equipos inscritos).
5. El Usuario selecciona una liga de la lista si desea ver su detalle completo.

#### Escenarios Excepcionales

* **3a. El Usuario no ha creado ni pertenece a ninguna liga:**
  1. El Sistema detecta que no existen registros asociados al ID del Usuario.
  2. El Sistema muestra un mensaje informativo notificando que no se encontraron ligas activas ("No estás inscrito en ninguna liga").
  3. El Sistema ofrece accesos directos para 'Crear Liga' o 'Buscar Ligas Disponibles'.

* **3b. Error de conexión con la base de datos o token caducado:**
  1. El Sistema no logra recuperar los datos del servidor.
  2. El Sistema muestra un mensaje de error notificando la falla de carga y permite al usuario reintentar.