En este documento se explica el proceso de normalización de una base de datos para un punto de venta
de articulos de baño (tinas, retretes, lavamanos, etc.) a partir de una base de datos existente.

El archivo base de DB_original.png muestra la el diagrama relacional de la base de datos original
a partir de este diagrama se realizarán las modificaciones necesarias.

![Base de datos original](https://github.com/ReneGarBer/SQL_projects/blob/main/database_normalizacion_testing/SQL_projects/database_normalizacion_testing/DB_original.png)

1º El primer paso es crear un diccionario de datos que nos sirva de guía para entender que representan 
cada uno de los atributos de cada una de las tablas, esto nos ayuda a comprender mejor las relaciones 
entre las tablas y las dependencias funcionales que pueden existir entre los atributos de las mismas.
El diccionario de datos deberá modificarse conforme se altere el diagrama entidad relación.

