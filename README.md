# DBA-CASOS
## Caso F
Observar el comportamiento de DeadLock Anidado: 
Teoría: Durante la ejecución de transacciones cuando los recursos son utilizados y la transacción no libera los datos que está utilizando, se generará una Error debido a que en  caso que otra transacción desea interactuar con los datos que la transación previa está utilizando.

El siguiente ejemplo describe el caso de las llamadas de transacciones anidadas, es decir una transacción llama a la definición de otras transacciones, el ejemplo define las siguientes transacciones: 

Una transacción A: Que realiza una actualización de los datos de la Tabla Productos, espera un delay de  10 segundos, hace un llamado a la transacción D.
Una Transacción B: Realiza una llamada de la transacción A.
Una Transacción C: Que realiza una llamada a la transacción B.
Una Transacción D: Que realiza una llamada a la transacción A y esoera un delay de 10 segundos.

## Definición de las bases de datos:
Tabla Productos: Conformado por un identificador único de tipo entero y un nombre:
```SQL
CREATE TABLE Productos (
    id INT PRIMARY KEY,
    name VARCHAR(30)
)
```

Tabla Empleados: Conformado por un identificador único de tipo entero y un nombre
```SQL
CREATE TABLE Empleados (
    id INT PRIMARY KEY,
    name VARCHAR(30)
)
```

Tabla Tiendas: Conformado por un identificador único de tipo entero y un nombre
```SQL
CREATE TABLE Tiendas (
    id INT PRIMARY KEY,
    name VARCHAR(30)
)
```

## Definción de la transacción: 
Observar que algunas transacciones no tienen definido la operación: [COMMIT tran](https://stackoverflow.com/questions/4896479/what-happens-if-you-dont-commit-a-transaction-to-a-database-say-sql-server?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
### Transacción A
```SQL
GO  
CREATE PROCEDURE TransacA   
       
AS   
BEGIN TRAN
    PRINT N'EXEC Transac A'
    WAITFOR DELAY '00:10:00'
    EXEC TransacD
GO 
```
### Transacción B
```SQL
GO  
CREATE PROCEDURE TransacB  
       
AS   
BEGIN TRAN
    PRINT N'EXEC Transac B'
    EXEC TransacA
COMMIT TRAN
GO 
```
### Transacción C
```SQL
GO  
CREATE PROCEDURE TransacC
       
AS   
BEGIN TRAN
    PRINT N'EXEC Transac C'
    EXEC TransacB
COMMIT TRAN
GO 
```
### Transacción D
```SQL
GO  
CREATE PROCEDURE TransacD
       
AS   
BEGIN TRAN
    PRINT N'EXEC Transac D'
    EXEC TransacB
COMMIT TRAN
GO 
```


## Uso la forma de delay:
Se define un delay de 5 a 10 segundos.


```SQL
  WAITFOR DELAY '00:05:00';  
```

## Comportamiento de la transacción:
Se ejecuta la transacción C desde una Conexión 1, la cual llama a  la transacción B, que luego llama a la transacción A y luego D, que llama a A.
```SQL
 exec TransacC
```
Se ejecuta desde otra conexión a la Transacción D que llama a A de forma recursiva.

```SQL
 exec TransacD
```
Como la aplicación no tiene una transacción en la cual se hace  un commit  o un rollback se corre el riesgo de  que el registro que se esté utilizando para la actualización retorne un error de tipo: Deadlock


## Error esperado:

[Referencia](https://msdn.microsoft.com/en-us/library/aa266504(v=vs.60).aspx)


## Definición: SQL Query:
```SQL
INSERT INTO Tiendas (id,namme) VALUES (1,' T 1');
INSERT INTO Tiendas (id,namme) VALUES (2' T 2');
INSERT INTO Tiendas (id,namme) VALUES (3' T 3');


INSERT INTO Productos (id,name) VALUES (1,' P 1');
INSERT INTO Productos (id,namme) VALUES (2' P 2');
INSERT INTO Productos (id,namme) VALUES (3' P 3');


INSERT INTO Empleados (id,name) VALUES (1,' E 1');
INSERT INTO Empleados (id,namme) VALUES (2' E 2');
INSERT INTO Empleados (id,namme) VALUES (3' E 3');


```
