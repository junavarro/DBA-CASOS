# DBA-CASOS
##Caso F
Observar el comportamiento de DeadLock Anidado: 
Teoría: Durante la ejecución de transacciones cuando los recursos son utilizados y la transacción no libera los datos que está utilizando, se generará una Error debido a que en  caso que otra transacción desea interactuar con los datos que la transación previa está utilizando.

El siguiente ejemplo describe el caso de las llamadas de transacciones anidadas, es decir una transacción llama a la definición de otras transacciones, el ejemplo define las siguientes transacciones: 

Una transacción A: Que realiza una actualización de los datos de la Tabla Productos, espera un delay de  10 segundos, hace un llamado a la transacción D.
Una Transacción B: Realiza una llamada de la transacción A.
Una Transacción C: Que realiza una llamada a la transacción B.
Una Transacción D: Que realiza una llamada a la transacción A y esoera un delay de 10 segundos.

##Definición de las bases de datos:
Tabla Productos: Conformado por un identificador único de tipo entero y un nombre
Tabla Empleados: Conformado por un identificador único de tipo entero y un nombre
Tabla Tiendas: Conformado por un identificador único de tipo entero y un nombre

##Definción de la transacción:

##Uso la forma de delay:
##Comportamiento de la transacción:
##Error esperado:
##Definición: SQL Query:
