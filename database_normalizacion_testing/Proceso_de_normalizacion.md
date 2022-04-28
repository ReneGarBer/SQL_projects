En este documento se explica el proceso de normalización de una base de datos para un punto de venta
de articulos de baño (tinas, retretes, lavamanos, etc.) a partir de una base de datos existente.

El archivo base de DB_original.png muestra la el diagrama relacional de la base de datos original
a partir de este diagrama se realizarán las modificaciones necesarias.

![Base de datos original](https://github.com/ReneGarBer/SQL_projects/blob/main/database_normalizacion_testing/DB_original.png)

1º El primer paso es crear un diccionario de datos que nos sirva de guía para entender que representan 
cada uno de los atributos de cada una de las tablas, esto nos ayuda a comprender mejor las relaciones 
entre las tablas y las dependencias funcionales que pueden existir entre los atributos de las mismas.
El diccionario de datos deberá modificarse conforme se altere el diagrama entidad relación.

** Empleado (Contiene la información que identifica a los empleados) **
| Nombre | Tipo	| Restricción |	Descripción |
|--------|------|-------------|-------------|
| Id	| Entero |	Serial, no nulo |	Número único para identificar la fila |
| Nombre | Cadena | No nulo	| Nombre que aparece en el acta de nacimiento del empleado |
| Apellido_1 | Cadena	| No nulo	Primer apellido que aparece en el acta de nacimiento del empleado |
| Apellido_2 | Cadena	 | Segundo apellido que aparece en el acta de nacimiento del empleado |
| Nombre_usuario | Cadena	| No nulo	Nombre que usara el empleado para identificarse en la aplicación |
| Email	| Cadena | No nulo | Correo electrónico del empleado |
| Contraseña | Cadena	| No nulo |	Contraseña que usará el empleado para acceder a la aplicación |
| Puesto |Cadena | No nulo | Cargo que ocupa el empleado dentro de la empresa |
| Salario |	Decimal	No nulo	| Sueldo del empleado |
|Activo	| Booleano |	No nulo | Es verdadero si el empleado continua trabajando en la empresa, de lo contrario es falso. |

