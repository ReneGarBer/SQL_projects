En este documento se explica el proceso de normalización de una base de datos para un punto de venta
de articulos de baño (tinas, retretes, lavamanos, etc.). Se quiere, a partir de una base de datos existente, crear una aplicación
que sirva para el control de inventario y personal de la empresa.

El archivo base de DB_original.png muestra la el diagrama relacional de la base de datos original
a partir de este diagrama se realizarán las modificaciones necesarias.

![Base de datos original](https://github.com/ReneGarBer/SQL_projects/blob/main/database_normalizacion_testing/DB_original.png)

1º El primer paso es crear un diccionario de datos que nos sirva de guía para entender que representan 
cada uno de los atributos de cada una de las tablas, esto nos ayuda a comprender mejor las relaciones 
entre las tablas y las dependencias funcionales que pueden existir entre los atributos de las mismas.
El diccionario de datos deberá modificarse conforme se altere el diagrama entidad relación.

**Empleado (Contiene la información que identifica a los empleados)**
| Nombre | Tipo	| Restricción |	Descripción |
| ------ | ---- | ----------- | ----------- |
| Id	| Entero |	Serial, no nulo |	Número único para identificar la fila |
| Nombre | Cadena | No nulo	| Nombre que aparece en el acta de nacimiento del empleado |
| Apellido_1 | Cadena	| No nulo	| Primer apellido que aparece en el acta de nacimiento del empleado |
| Apellido_2 | Cadena	 | |Segundo apellido que aparece en el acta de nacimiento del empleado |
| Nombre_usuario | Cadena	| No nulo	| Nombre que usara el empleado para identificarse en la aplicación |
| Email	| Cadena | No nulo | Correo electrónico del empleado |
| Contraseña | Cadena	| No nulo |	Contraseña que usará el empleado para acceder a la aplicación |
| Puesto |Cadena | No nulo | Cargo que ocupa el empleado dentro de la empresa |
| Salario |	Decimal	No nulo	| Sueldo del empleado |
|Activo	| Booleano |	No nulo | Es verdadero si el empleado continua trabajando en la empresa, de lo contrario es falso. |

**Ticket_venta (Representa el ticket que se generará al hacer la venta)**
| Nombre	| Tipo	| Restricción |	Descripción |
| ------- | ----  | ----------- | ----------- |
| Folio	| Entero	| Serial no nulo | Número único que sirve para registrar cada venta |
| Fecha	| Timestamp |	No nulo	| Fecha en la que se hizo la venta formato “dd/mm/aaaa hr:min:seg” |
| Id_empleado	| Entero | No nulo | Id del empelado que hizo la venta |
| RFC_Cliente |	Cadena | |RFC del cliente |

**Ticket_compra (ticket de la compra de inventario hecha a los proveedores)**
| Nombre | Tipo	| Restricción	| Descripción |
| ------ | ---- | ----------- | ----------- |
| Folio	| Entero	| No nulo	| Folio del ticket generado por el proveedor |
| Fecha	| Timestamp |	No nulo	| Fecha de la compra |
| Id_empleado	| Entero | No nulo | Id del empleado que realizó la compra |
| Id_proveedor | Cadena | No nulo | Id del proveedor |

**Venta (Lista de artículos que se vendieron)**
| Nombre | Tipo	| Restricción |	Descripción |
| ------ | ---- | ----------- | ----------- |
| Id | Entero	| No nulo, serial |	Entero que identifica a cada fila de la tabla |
| Folio	| Entero | No nulo | Referencia a la columna Folio de la tabla Ticket_venta |
| Producto |Entero | No nulo |	Referencia a la columna Id de la tabla Producto |
| Color	| Cadena | No nulo	| Indica el color del producto |
| Cantidad | Entero | No nulo, Mayor a cero |	Cantidad de productos vendidos |

**Compra (Lista de artículos comprados)**
| Nombre | Tipo	| Restricción |	Descripción |
| ------ | ---- | ----------- | ----------- |
| Id | Entero	| No nulo, Serial	| Entero que identifica a cada fila de la tabla |
| Folio	| Entero | No nulo | Referencia a la columna Folio de la tabla Ticket_compra |
| Producto | Entero	| No nulo	| Referencia a la columna Id de la tabla Producto |
| Precio | Decimal | No nulo | Indica el precio de compra del producto |
| Cantidad | Entero	| No nulo, mayor a cero	| Cantidad de productos comprados |

**Proveedor (Tabla con la información de los distintos proveedores)**
| Nombre | Tipo | Restricción |	Descripción |
| ------ | ---- | ----------- | ----------- |
| Id | Entero	| No nulo, Serial	| Entero que identifica a cada fila de la tabla |
| RFC	| Cadena | No nulo	| RFC del proveedor |
| Nombre | Cadena	| No nulo	| Nombre del proveedor |
| Telefono | Cadena	| No nulo, contener solo números | Número de teléfono del proveedor |
| Direccion |	Cadena |	| Dirección física del proveedor |
| Activo | Booleano |	No nulo |	Verdadero si el proveedor sigue activo, de lo contrario es falso |

**Producto (Información de los productos que se venden en la tienda)**
| Nombre | Tipo | Restricción	| Descripción |
| ------ | ---- | ----------- | ----------- |
| Id |Entero | No nulo, serial |	Entero que identifica a cada fila de la tabla |
| Tipo | Cadena	| No nulo	| Tipo de producto, ejemplos: tina, lavamanos, cortinas. |
| Nombre |	Cadena | No nulo | Nombre del producto |
| Precio |	Decimal	| No nulo	| Precio de compra del producto |
| Descripcion |	Cadena | | Descripcion del producto |
| Existencia |	Entero | | Cantidad de producto disponible para venta |
| Reusable |	Booleano | No nulo | Verdadero si el producto es reusable, falso de lo contrario |

**Normalización**
La normalización puede entenderse como el proceso de análisis basado en dependencias funcionales y claves principales para minimizar la redundancia y anomalías de inserción, borrado y actualización.

Anomalías de inserción: No poder agregar atributos a causa de otros atributos que no deberían tener dependencias funcionales.

Anomalías de modificación: Al modificar datos duplicados no se modifican todas las intancias.

Anomalías de eliminación: Se pierden algunos atributos por eliminar otros.

Dependencia Funcional:

Ahora buscamos casos en los que pudieramos incurrir en esas anomalías.
1º Anomalias de inserción. El atributo Puesto de la tabla Empleado puede darnos problemas, supongamos que contamos con el puesto 'vendedor', no se hace distinción 
si vende en la tienda física o de manera telefónica, la empresa quiere dividir estas áreas por cuestiones de crecimiento y delegar responbsabilidades a más empleados. Los vendedores actuales de la empresa pasaran a ser vendedor_en_tienda (los que trabajan en las tiendas físicas) y vendedor_telefono, estos últimos están por contratarse. 
Nos sería imposible agregar el nuevo puesto vendedor_telefono debido a que tiene una dependencia funcional con el atributo id de la tabla Empleado, se necesita un empleado para poder agregarlo a la tabla con el puesto vendedor_telefono.

2º Anomalías de modificación: Supongamos que queremos modificar el atributo Tipo de la tabla Producto, distintas celdas de esta columna tiene escrito el atributo de distinta forma, el atributo debería ser 'Bañera', en cambio tenemos 'tina', 'tina de baño', 'Tina para baño'. Queremos moficar todos estos tipos con una sola transacción, esto aunque no es imposible de hacer, se vuelve complicado debido a que tendríamos que incluir cada caso en las condiciones de nuestra conculta, ejemplo: ' ... Where Tipo = 'tina' OR Tipo = 'tina de baño' OR Tipo = 'tina para baño';' esto no garantiza que se corrija el error ya que es posible que algún caso se nos haya pasado por alto.

3º Podemos observarlo en el caso de el atributo Puesto de la tabla Empleado, si se elimina de la tabla, ya sea por despido o renuncia, a un empleado quien era el único en ocupar un puesto en particular, perdemos toda la información relacionada a este puesto.

De esta manera determinamos que la base de datos sí necesita ser normalizada.
