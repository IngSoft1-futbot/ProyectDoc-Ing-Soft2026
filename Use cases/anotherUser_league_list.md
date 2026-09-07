### Caso de uso: Listar ligas de otro usuario

#### Descripción
Permite a un usuario consultar la lista de ligas en las que participa o ha creado otro usuario en particular al visualizar su perfil.

#### Atributos
* **Actor Primario:** Usuario (Consultante)
* **Actor Secundario:** Sistema
* **Precondición:** 
  1. El Usuario consultante debe estar autenticado en la plataforma.
  2. El usuario objetivo (del cual se quieren consultar las ligas) debe estar registrado en el sistema.

#### Inputs
1. ID o Nombre del usuario objetivo a consultar.

#### Escenario Exitoso
1. El Usuario navega al perfil de otro usuario o selecciona su nombre en una lista/búsqueda.
2. El Usuario presiona la pestaña o sección "Ligas de [Nombre de Usuario]".
3. El Sistema consulta en la base de datos las ligas públicas donde el usuario objetivo figura como creador o participante.
4. El Sistema obtiene y procesa los registros.
5. El Sistema muestra en pantalla el listado de ligas del usuario consultado (Nombre de la liga, Estado, Rol del usuario [Creador/Participante] y N° de equipos).
6. El Usuario puede hacer clic en cualquiera de las ligas mostradas para ver sus detalles públicos.

#### Escenarios Excepcionales

* **3a. El usuario consultado no participa en ninguna liga:**
  1. El Sistema detecta que no existen registros de ligas asociadas a ese usuario.
  2. El Sistema muestra un mensaje informativo indicando "Este usuario no pertenece a ninguna liga actualmente".

* **3b. Las ligas del usuario consultado son privadas:**
  1. El Sistema detecta que las ligas del usuario tienen visibilidad restringida o privada.
  2. El Sistema muestra únicamente los nombres con un indicador de "Liga Privada", impidiendo el acceso a sus detalles sin clave.