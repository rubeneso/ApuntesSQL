# Consultas SQL
## Sentencias y filtros
### Índice
- [Sentencia SELECT](#sentencia-select)
- [Instrucción WHERE](#instrucción-where)
- [Funciones](#funciones)

### Sentencia SELECT
La sentencia de consulta de SQL es `SELECT`.
Se usa para seleccionar los campos de los que queramos recuperar la información. Después de ella debe ir la sentencia `FROM`, que indica la tabla de la que sacamos los valores. Un ejemplo sería el siguiente:
```sql
SELECT nombre, dni
FROM alumnos;
```
Los campos a recuperar se escriben uno a uno separados por comas. Para recuperar todos los campos de la tabla usamos el asterisco "*". Si en nuestro programa quisieramos llamar a un campo de otra manera se puede usar `AS`. Por ejemplo, si después del `SELECT` ponemos `nombre AS nom` pasaríamos a poder usar "nom" para referirnos a "nombre" en todo el resto del programa.

### Instrucción WHERE

Si solo queremos recuperar los campos que cumplan una determinada condición usamos `WHERE`. Si quisieramos consultar a todos los alumnos que se llamaran Bernardo, escribiriamos:
```sql
SELECT nombre
FROM alumnos
WHERE nombre = 'Bernardo';
```
Podemos poner varias condiciones concatenandolas con `AND` o con `OR`:
```sql
SELECT nombre
FROM alumnos
WHERE nombre = 'Bernardo'
  OR nombre = 'Alicia';
```
Operadores para las condiciones: 
* Igual que: = o LIKE
* Menor que: <
* Menor o igual que: <=
* Mayor que: >
* Mayor o igual que: >=
* Distinto: <> o IS NOT
* Nulo: null o IS NULL

Otras posibles condiciones del `WHERE` son:
* `IN`: especificamos los valores que queremos que aparezcan en un campo:
```sql
SELECT nombre
FROM alumnos
WHERE nombre IN ('Alicia', 'Bernardo', 'Eva');
```
* `BETWEEN`: valores que estén dentro de un determinado rango, incluyendo los extremos:
```sql
SELECT nombre, edad
FROM alumnos
WHERE edad BETWEEN 12 AND 16;
```
* `LIKE`: valores que siguen un patrón. Para ello podemos usar:
  * "%", reemplaza 0 o n caracteres
  * "_" reemplaza a un caractér
```sql
SELECT nombre
FROM alumnos
WHERE nombre LIKE 'R_%';
```
> Todos los nombres que empiezen por "R" y tengan pegada a esa "R" un caractér o más

### Funciones
Para ayudarnos a la hora de programar existen unas funciones que podemos usar para resolver ciertos problemas:
* `CONCAT()`:
