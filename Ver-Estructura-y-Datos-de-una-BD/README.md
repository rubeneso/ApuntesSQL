# Comandos para ver la estructura y los datos de una BD
## Ver los Schemas que hay en el servidor:
```sql
SHOW DATABASES;
```

![SHOW DATABASES](img/showDatabases.png)

## Ver las tablas que hay en el Schema activo:
```sql
SHOW TABLES;
```

![SHOW DATABASES](img/showDatabases.png)

## Ver las columnas y los detalles de una tabla:
```sql
(SHOW CREATE TABLE <nombre_tabla>; | DESCRIBE <nombre_tabla>;)
```

![SHOW CREATE TABLE](img/showCreateTable.png)
![DESCRIBE TABLE](img/describeTable.png)
