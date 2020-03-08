# DDL - Definición de la estructura
**DDL** (Data Definition Language) es el sub-lenguaje de SQL que agrupa los comandos necesarios para crear, modificar y borrar las estructuras de SQL como pueden ser los esquemas o las tablas

>**¡OJO!** Esta sección está documentada para el gestor de bases de datos **PostgreSQL**, las instrucciones y su sintaxis pueden ser diferentes en otro gestor de bases de datos

### Índice
 - [Instrucción CREATE](#instrucci%c3%b3n-create)
   - [CREATE SCHEMA](#create-schema)
   - [CREATE TABLE](#create-table) 
     - [Tipos de datos - Dominio](#tipos-de-datos---dominio)
     - [Restricciones de atributos (Constraints)](#restricciones-de-atributos-constraints)

## Instrucción CREATE
Esta es la instrucción que usaremos para crear elementos en nuestra base de datos. Podemos crear dos elementos principalmente, un schema y una tabla:
### CREATE SCHEMA
Un schema dentro de nuestra base de  datos es una colección de elementos (tablas principalmente). La sintaxis es la siguiente:
```sql
CREATE SCHEMA [IF NOT EXISTS] <nombre_schema>;
```
> "IF NOT EXISTS" hace que no se cree el schema si ya existe uno con el mismo nombre. Que aparezca entre corchetes significa que es opcional ponerlo

### CREATE TABLE
Las tablas en las bases de datos son los elementos que guardan los atributos y sus valores de una determinada entidad. En el momento de crear la tabla se definen los atributos que va a tener y a que tipo de datos pertenecen (dominio). Opcionalmente se pueden incluir restricciones, que se explican más adelante. La sintaxis es la siguiente:
```sql
CREATE TABLE [IF NOT EXISTS] <nombre_de_la_tabla>(
  <nombre_atributo_1> <dominio_atributo_1> <restricción_1> <restricción_2> ...,
  <nombre_atributo_2> <dominio_atributo_2> <restricción_1> <restricción_2> ...,
  ...
);
```
```sql
CREATE TABLE IF NOT EXISTS alumnos(
  dni CHAR(9),
  nombre VARCHAR(30),
  edad INTEGER,
  fNacimiento DATE
);
```
> Ejemplo de creación de una tabla sin restricciones en los atributos
#### Tipos de datos - Dominio
Los dominios más habituales para los atributos en PostgreSQL son los siguientes:
* Numéricos:
  * Integer: número entero, sin decimales de -2147483648 a +2147483647
  * Bigint: de -9223372036854775808 a +9223372036854775807
  * Smallint de -32768 a +32767
  * Decimal: preciso, 131072 digitos antes de la coma, 16282 dígitos después de la coma
  * Real: no preciso, máximo 6 decimales, de -2147483648 a +2147483647
  * Double: máximo 15 decimales, de -9223372036854775808 a +9223372036854775807
* Texto:
  * Char(<longitud_total>): longitud fija y limitada
  * Varchar(<longitud_maxima>): longitud variable y limitada
  * Text: longitud variable e ilimitada
* Fecha:
  * Date: dia, mes, año
  * Time: horas, minutos, segundos
  * Timestamp: date + time
* Boleano:
  * Boolean: true o false
* Dinero:
  * Money
* Direcciones de red:
  * Inet: para direcciones host IPv4 e IPv6
  * Cidr: para direcciones de red IPv4 e IPv6

#### Restricciones de atributos (Constraints)
//TODO