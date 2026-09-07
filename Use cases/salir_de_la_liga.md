### USE CASE 26: SALIR DE LA LIGA

### PRIMARY ACTOR:
Usuario

### SECONDARY ACTOR:
El Sistema

### PRECONDITION:
1. Estar en la liga
2. Estar logueado

### Succesfull scenarios:
1. El Usuario hace click en panel de Ligas
2. El Sistema responde desplegando con un panel con toda la informacion referida a la Liga
3. El Usuario decide eliminar a su equipo de la liga
4. El Sistema responde borrando al Usuario de esa liga, y actualiza la Base de Datos

### Aternative scenarios:
1. El Usuario no hace click en el panel de ligas
2. Flow: El Sistema no despliega el panel con la informacion de la Liga
3. El Usuario decide no eliminar a su equipo de la liga
4. Flow: El Sistema no borra al Usuario de esa Liga, y no actualiza la Base de datos con nueva informacion

### Exeptional scenarios:
1. *** Error de conexion a Internet ***
1. Flow: El Sistema muestra un mensaje de error

### POSTCONDITION:
1. El Usuario salio de la Liga a la cual pertenecia





