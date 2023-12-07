### CREACIÓN DE DATABASE

------

```sql
DROP DATABASE IF EXISTS TMB;
CREATE DATABASE TMB
USE TMB;
```
### ELIMINAR TABLAS ANTERIORES  DE DATABASE

------

```
DROP TABLE IF EXISTS zonas;
DROP TABLE IF EXISTS ruta;
DROP TABLE IF EXISTS parada_ruta;
DROP TABLE IF EXISTS conductor;
DROP TABLE IF EXISTS bus;
DROP TABLE IF EXISTS Estacion;
DROP TABLE IF EXISTS recorrido;
```

### CREACIÓN DE TABLAS

------

```sql
CREATE TABLE zonas(  
    id_zona int NOT NULL PRIMARY KEY AUTO_INCREMENT COMMENT 'Primary Key',
    nombre_zona VARCHAR(30)
) 

CREATE TABLE ruta (
    id_ruta int NOT NULL PRIMARY KEY AUTO_INCREMENT COMMENT 'Primary Key',
    nombre_ruta VARCHAR(30),
    tiempo_ruta TIME,
    id_zona int NOT NULL,
    FOREIGN KEY (id_zona) REFERENCES zonas(id_zona)
)


CREATE TABLE Estacion(
    id_estacion int NOT NULL PRIMARY KEY AUTO_INCREMENT COMMENT 'Primary Key',
    nombre_estacion VARCHAR(30)
)

CREATE TABLE parada_ruta (
    id_ruta int NOT NULL,
    id_estacion int NOT NULL,
    FOREIGN KEY (id_ruta) REFERENCES ruta(id_ruta),
    FOREIGN KEY (id_estacion) REFERENCES Estacion(id_estacion)
)

CREATE TABLE bus (
    id_bus int NOT NULL PRIMARY KEY AUTO_INCREMENT COMMENT 'Primary Key',
    placa VARCHAR(30) NOT NULL
)

CREATE TABLE conductor (
    id_conductor int NOT NULL PRIMARY KEY AUTO_INCREMENT COMMENT 'Primary Key',
    nombre_conductor VARCHAR(30) NOT NULL
)

CREATE TABLE recorrido (
    id_conductor int NOT NULL,
    id_bus int NOT NULL,
    id_ruta int,
    dia VARCHAR(15),
    FOREIGN KEY (id_conductor) REFERENCES conductor(id_conductor),
    FOREIGN KEY (id_bus) REFERENCES bus(id_bus),
    OREIGN KEY (id_ruta) REFERENCES ruta(id_ruta);
)
```

### INFORMACIÓN PARA POBLAR

------

```sql
INSERT INTO zonas (nombre_zona)
VALUES 'Norte'),('Sur'),('Oriente'),('Occidente'),('Floridablanca'),('Girón'),('Piedecuesta');

INSERT INTO bus (placa)
VALUES
    ("XVH345"),
    ("XDL965"),
    ("XFG847"),
    ("XRJ452"),
    ("XDF459"),
    ("XET554"),
    ("XKL688"),
    ("XXL757");

INSERT INTO conductor (nombre_conductor)
VALUES
    ("Andrés Manuel López Obrador"),
    ("Nicolás Maduro Moros"),
    ("Alberto Fernández"),
    ("Luiz Inácio Lula da Silva"),
    ("Gabriel Boric"),
    ("Miguel Díaz-Canel"),
    ("Daniel Ortega"),
    ("Gustavo Petro Urrego"),
    ("Luis Arce"),
    ("Xiomara Castro");

INSERT INTO Estacion (nombre_estacion)
VALUES ("Colseguros"), ("Clínica Chicamocha"), ("Plaza Guarín"), ("Mega Mall"), ("UIS"), ("UDI"), ("Santo Tomás"), ("Boulevard Santander"), ("Búcaros"), ("Rosita"), ("Puerta del Sol"), ("Cacique"), ("Plaza Satélite"), ("La Sirena"), ("Provenza"), ("Fontana"), ("Gibraltar"), ("Terminal"), ("Mutis"), ("Plaza Real");

INSERT INTO ruta (nombre_ruta,tiempo_ruta,id_zona)
VALUES ("Universidades", '2:00:00',1), ("Café Madrid", '2:15:00',2), ("Cacique", '1:45:00',2), ("Diamantes", '1:50:00',4), ("Terminal", '2:00:00',4), ("Prado", '1:30:00',3), ("Cabecera", '2:00:00',3), ("Ciudadela", '2:00:00',4), ("Punta Estrella", '2:30:00',7), ("Niza", '2:45:00',5), ("Autopista", '2:15:00',5), ("Lagos", '2:15:00',5), ("Centro Florida", '2:30:00',5);
INSERT INTO parada_ruta (id_ruta,id_estacion)
VALUES(1,1) 
(1,2),
(1,3),
(1,4),
(1,5),
(1,6),
(1,7),
(3,8),
(3,9),
(3,10),
(3,11),
(3,12),
(4,13),
(4,14),
(4,15),
(5,16),
(5,17),
(8,18),
(8,19),
(8,20);
INSERT INTO recorrido(id_conductor, id_bus, dia, id_ruta)
VALUES 
    (5, 1, "Lunes", 1),
    (5, 1, "Martes", 1),
    (5, 3, "Miércoles", 1),
    (5, 3, "Jueves", 1),
    (5, 5, "Viernes", 2),
    (5, 5, "Sábado", 2),
    (5, 5, "Domingo", 2),
    (3, 5, "Lunes", 4),
    (3, 6, "Martes", 4),
    (3, 1, "Miércoles", 4),
    (3, 1, "Jueves", 5),
    (3, 3, "Viernes", 5),
    (3, 3, "Sábado", 5),
    (3, 3, "Domingo", 5),
    (10, 3, "Lunes", 10),
    (10, 3, "Martes", 10),
    (10, 5, "Miércoles", 10),
    (10, 5, "Jueves", 10),
    (10, 4, "Viernes", 10),
    (10, 7, "Sábado", 11),
    (10, 7, "Domingo", 11),
    (7, 7, "Lunes", 11),
    (7, 7, "Martes", 11),
    (6, 7, "Miércoles", 12),
    (6, 7, "Jueves", 12),
    (6, 7, "Viernes", 12),
    (6, 6, "Sábado", 12),
    (6, 6, "Domingo", 12);



```

------

### CONSULTAS PROPUESTAS

1. Cantidad de paradas por ruta

   ```sql
   SELECT id_ruta, COUNT(id_ruta)  as NumeroDePardasPorRuta from parada_ruta GROUP BY id_ruta;
   +---------+-----------------------+
   | id_ruta | NumeroDePardasPorRuta |
   +---------+-----------------------+
   |       1 |                     7 |
   |       3 |                     5 |
   |       4 |                     3 |
   |       5 |                     2 |
   |       8 |                     3 |
   +---------+-----------------------+
   5 rows in set (0,00 sec)
   
   ```

   

2. Nombre de las paradas de la ruta universidades

   ```sql
   SELECT E.nombre_estacion as parada from Estacion E INNER JOIN parada_ruta PR on E.id_estacion = PR.id_estacion WHERE PR.id_ruta=1; 
   +---------------------+
   | parada              |
   +---------------------+
   | Colseguros          |
   | Clínica Chicamocha  |
   | Plaza Guarín        |
   | Mega Mall           |
   | UIS                 |
   | UDI                 |
   | Santo Tomás         |
   +---------------------+
   7 rows in set (0,00 sec)
   
   ```

3. Nombres de las Rutas No Programadas

   ```sql
   SELECT R.id_ruta, R.nombre_ruta as RutasNoProgramadas from ruta R where R.id_ruta NOT IN (SELECT DISTINCT id_ruta FROM recorrido);
   +---------+--------------------+
   | id_ruta | RutasNoProgramadas |
   +---------+--------------------+
   |       3 | Cacique            |
   |       6 | Prado              |
   |       7 | Cabecera           |
   |       8 | Ciudadela          |
   |       9 | Punta Estrella     |
   |      13 | Centro Florida     |
   +---------+--------------------+
   6 rows in set (0,00 sec)
   
   ```

4. Rutas Programadas sin Conductor Asignado

   ```sql
   SELECT R.nombre_ruta 
   FROM ruta R 
   INNER JOIN recorrido RE ON RE.id_ruta= R.id_ruta 
   INNER JOIN conductor C on C.id_conductor=RE.id_conductor 
   WHERE C.id_conductor IS NULL;
   
   Empty set (0,00 sec)
   
   ```

5. Conductores No Asignados a la Programación

   ```sql
   SELECT C.id_conductor, C.nombre_conductor as NombreDeConductoresNoAsignados from conductor C where C.id_conductor NOT IN (SELECT DISTINCT id_conductor FROM recorrido);
   +--------------+--------------------------------+
   | id_conductor | NombreDeConductoresNoAsignados |
   +--------------+--------------------------------+
   |            1 | Andrés Manuel López Obrador    |
   |            2 | Nicolás Maduro Moros           |
   |            4 | Luiz Inácio Lula da Silva      |
   |            8 | Gustavo Petro Urrego           |
   |            9 | Luis Arce                      |
   +--------------+--------------------------------+
   5 rows in set (0,00 sec)
   
   ```

6. Buses No asignados a la Programación

   ```sql
   SELECT B.id_bus, B.placa as BusNoAsignado from bus B where B.id_bus NOT IN (SELECT DISTINCT id_bus FROM recorrido);
   +--------+---------------+
   | id_bus | BusNoAsignado |
   +--------+---------------+
   |      2 | XDL965        |
   |      8 | XXL757        |
   +--------+---------------+
   2 rows in set (0,00 sec)
   
   
   ```

7. Zonas NO Programadas

   ```sql
   SELECT Z.id_zona, Z.nombre_zona as ZonasNoProgramadas from zonas Z where Z.id_zona NOT IN (SELECT DISTINCT id_zona FROM ruta);
   +---------+--------------------+
   | id_zona | ZonasNoProgramadas |
   +---------+--------------------+
   |       6 | Girón              |
   +---------+--------------------+
   1 row in set (0,00 sec)
   
   
   ```

8. Programación asignada a cada conductor (Conductor, Ruta y Día)

   ```sql
   SELECT C.nombre_conductor as NombreConductor, RE.id_ruta, RE.dia 
   FROM recorrido RE 
   INNER JOIN conductor C ON C.id_conductor= RE.id_conductor ;
   +--------------------+---------+------------+
   | NombreConductor    | id_ruta | dia        |
   +--------------------+---------+------------+
   | Gabriel Boric      |       1 | Lunes      |
   | Gabriel Boric      |       1 | Martes     |
   | Gabriel Boric      |       1 | Miércoles  |
   | Gabriel Boric      |       1 | Jueves     |
   | Gabriel Boric      |       2 | Viernes    |
   | Gabriel Boric      |       2 | Sábado     |
   | Gabriel Boric      |       2 | Domingo    |
   | Alberto Fernández  |       4 | Lunes      |
   | Alberto Fernández  |       4 | Martes     |
   | Alberto Fernández  |       4 | Miércoles  |
   | Alberto Fernández  |       5 | Jueves     |
   | Alberto Fernández  |       5 | Viernes    |
   | Alberto Fernández  |       5 | Sábado     |
   | Alberto Fernández  |       5 | Domingo    |
   | Xiomara Castro     |      10 | Lunes      |
   | Xiomara Castro     |      10 | Martes     |
   | Xiomara Castro     |      10 | Miércoles  |
   | Xiomara Castro     |      10 | Jueves     |
   | Xiomara Castro     |      10 | Viernes    |
   | Xiomara Castro     |      11 | Sábado     |
   | Xiomara Castro     |      11 | Domingo    |
   | Daniel Ortega      |      11 | Lunes      |
   | Daniel Ortega      |      11 | Martes     |
   | Miguel Díaz-Canel  |      12 | Miércoles  |
   | Miguel Díaz-Canel  |      12 | Jueves     |
   | Miguel Díaz-Canel  |      12 | Viernes    |
   | Miguel Díaz-Canel  |      12 | Sábado     |
   | Miguel Díaz-Canel  |      12 | Domingo    |
   +--------------------+---------+------------+
   28 rows in set (0,00 sec)
   
   ```

9. Programación asignada a conductores que hacen rutas de más de dos horas

   ```sql
   SELECT C.nombre_conductor as NombreConductor, RE.id_ruta, RE.dia 
   FROM recorrido RE 
   INNER JOIN conductor C ON C.id_conductor= RE.id_conductor
   INNER JOIN ruta R on R.id_ruta=RE.id_ruta
   WHERE R.tiempo_ruta>'02:00:00' ;
   +--------------------+---------+------------+
   | NombreConductor    | id_ruta | dia        |
   +--------------------+---------+------------+
   | Gabriel Boric      |       2 | Viernes    |
   | Gabriel Boric      |       2 | Sábado     |
   | Gabriel Boric      |       2 | Domingo    |
   | Xiomara Castro     |      10 | Lunes      |
   | Xiomara Castro     |      10 | Martes     |
   | Xiomara Castro     |      10 | Miércoles  |
   | Xiomara Castro     |      10 | Jueves     |
   | Xiomara Castro     |      10 | Viernes    |
   | Xiomara Castro     |      11 | Sábado     |
   | Xiomara Castro     |      11 | Domingo    |
   | Daniel Ortega      |      11 | Lunes      |
   | Daniel Ortega      |      11 | Martes     |
   | Miguel Díaz-Canel  |      12 | Miércoles  |
   | Miguel Díaz-Canel  |      12 | Jueves     |
   | Miguel Díaz-Canel  |      12 | Viernes    |
   | Miguel Díaz-Canel  |      12 | Sábado     |
   | Miguel Díaz-Canel  |      12 | Domingo    |
   +--------------------+---------+------------+
   17 rows in set (0,00 sec)
   
   ```

10. Nombres de Zonas y cantidad de rutas que tienen programadas (Contar)

    ```sql
    SELECT Z.nombre_zona as NombreZona, COUNT(RE.id_ruta) 
    FROM zonas Z 
    INNER JOIN ruta R on R.id_zona=Z.id_zona
    INNER JOIN recorrido RE ON RE.id_ruta= R.id_ruta
    GROUP BY Z.nombre_zona ;
    
    +---------------+-------------------+
    | NombreZona    | COUNT(RE.id_ruta) |
    +---------------+-------------------+
    | Norte         |                 4 |
    | Sur           |                 3 |
    | Occidente     |                 7 |
    | Floridablanca |                14 |
    +---------------+-------------------+
    4 rows in set (0,00 sec)
    
    ```

