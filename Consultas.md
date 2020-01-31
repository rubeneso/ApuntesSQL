# Consultas SQL
## Sentencias y filtros
### Índice
- [Sentencia SELECT](#sentencia-select)
- [Instrucción WHERE](#instrucción-where)
- [Funciones](#funciones)
  - [Funciones Reductoras](#funciones reductoras)
- [Instrucción GROUP BY](#instrucción-group-by)

### Sentencia SELECT
La sentencia de consulta de SQL es `SELECT`.
Se usa para seleccionar los campos de los que queramos recuperar la información. Después de ella debe ir la sentencia `FROM`, que indica la tabla de la que sacamos los valores. Un ejemplo sería el siguiente:

```sql
SELECT nombre, dni
FROM alumnos;
```
Los campos a recuperar se escriben uno a uno separados por comas. Para recuperar todos los campos de la tabla usamos el asterisco "*". Si en nuestro programa quisiéramos llamar a un campo de otra manera se puede usar `AS`. Por ejemplo, si después del `SELECT` ponemos `nombre AS nom` pasaríamos a poder usar "nom" para referirnos a "nombre" en todo el resto del programa.

### Instrucción WHERE

Si solo queremos recuperar los campos que cumplan una determinada condición usamos `WHERE`. Si quisiéramos consultar a todos los alumnos que se llamaran Bernardo, escribiríamos:
```sql
SELECT nombre
FROM alumnos
WHERE nombre = 'Bernardo';
```
Podemos poner varias condiciones concatenándolas con `AND` o con `OR`:
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
  * "_" reemplaza a un carácter
```sql
SELECT nombre
FROM alumnos
WHERE nombre LIKE 'R_%';
```
> Todos los nombres que empiecen por "R" y tengan pegada a esa "R" un carácter o más

### Funciones
Para ayudarnos a la hora de programar existen unas funciones que podemos usar para resolver ciertos problemas:

* `CONCAT()`: concatena cadenas de texto o valores de un campo
```sql
SELECT name
FROM world
WHERE capital LIKE CONCAT(name, ' City')
```

> Muestra las ciudades donde la capital sea igual que el nombre del país mas ' City'

* `REPLACE()`:  sirve para reemplazar caracteres dentro de una cadena. Requiere tres argumentos, el primero será la cadena en la que se va a reemplazar algo, el segundo los caracteres a reemplazar y el tercero los caracteres que se ponen en el lugar de los anteriores

```sql
SELECT REPLACE(name, 'a', '')
FROM world
```

> Esta consulta mostraría la lista de países eliminando la letra 'a' de todos ellos

* `ROUND()`:  redondea el número que se le pasa como primer parámetro. El segundo parámetro será el número de decimales a mostrar. Si el número es negativo se trunca el resultado en decenas (-1), centenas (-2)...
* `LENGTH()`: devuelve el largo de la cadena que le pasamos como parámetro
* `LEFT()`: devuelve una sub-cadena de la cadena que se le pasa como primer parámetro. El segundo parámetro se usa para indicar el número de posiciones que se cogen de la cadena original

#### Funciones reductoras

Este tipo de funciones van a reducir el número de tuplas resultado. Habitualmente estas funciones van a usarse junto con un `GROUP BY`. Todas estas funciones ignoran los valores **NULL**

* `SUM()`: suma el contenido de las tuplas del atributo que le pasemos por parámetro
* `COUNT()`: cuenta el número de tuplas con valor distinto a NULL del atributo que le pasemos por parámetro
* `AVG()`: hace el promedio del contenido de las tuplas del atributo que pasemos por parámetro

### Instrucción GROUP BY

Con esta instrucción podemos agrupar en sub-tablas los valores repetidos de un atributo. Por ejemplo, tenemos una tabla con dos atributos, países y continente al que pertenece cada uno. El resultado de agrupar esta tabla por continente sería una sub-tabla por cada uno de los distintos continentes, que tendría los países de cada uno. Una vez agrupados en sub-tablas las funciones reductoras se ejecutarían en cada sub-tabla, pudiendo por ejemplo contar el número de países que hay en cada continente con la función `COUNT()`. 

```sql
SELECT COUNT(name) AS 'Nª paises', continent AS 'Continente'
FROM world
GROUP BY continent
```

#### HAVING

