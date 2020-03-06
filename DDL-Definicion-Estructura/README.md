# DDL - Definición de la estructura
**DDL** (Data Definition Language) es el sub-lenguaje de SQL que agrupa los comandos necesarios para crear, modificar y borrar las estructuras de SQL como pueden ser los esquemas o las tablas

>**¡OJO!** Esta sección está documentada para el gestor de bases de datos **PostgreSQL**, las instrucciones y su sintaxis pueden ser diferentes en otro gestor de bases de datos

### Índice
 - [Instrucción CREATE](#instrucci%c3%b3n-create)
   - [CREATE SCHEMA](#create-schema)
   - [CREATE TABLE](#create-table) 

## Instrucción CREATE
Esta es la instrucción que usaremos para crear elementos en nuestra base de datos. Podemos crear dos elementos principalmente, un schema y una tabla:
### CREATE SCHEMA
Un schema dentro de nuestra base de  datos es una colección de elementos (tablas principalmente). La sintaxis es la siguiente:
```sql
CREATE (SCHEMA|DATABASE) [IF NOT EXISTS] <nombre_schema>;
```
> La tubería "|" representa opcionalidad entre "SCHEMA" y "DATABASE"

> "IF NOT EXISTS" hace que no se cree el schema si ya existe uno con el mismo nombre. Que aparezca entre corchetes significa que es opcional ponerlo

### CREATE TABLE
//TODO 