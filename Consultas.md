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
Es importante recalcar que todas las consultas en SQL se terminan con un punto y coma, es una regla del lenguaje. Los campos a recuperar se escriben uno a uno separados por comas. Para recuperar todos los campos de la tabla usamos el asterisco "*". Si en nuestro programa quisiéramos llamar a un campo de otra manera se puede usar `AS`. Por ejemplo, si después del `SELECT` ponemos `nombre AS nom` pasaríamos a poder usar "nom" para referirnos a "nombre" en todo el resto del programa.

### Instrucción WHERE

Si solo queremos recuperar las tuplas que cumplan una determinada condición usamos `WHERE`. Seguido del `WHERE` se pone un predicado, que se define como una función que devuelve "true" o "false". Esta instrucción se va a recorrer todas las tuplas de la tabla y solo devolverá aquellas tuplas que cumplan con las condiciones del predicado. Por ejemplo, si quisiéramos buscar a todos los alumnos que se llamaran Bernardo, escribiríamos:
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
```sql
SELECT nombre, edad
FROM alumnos
WHERE nombre = 'Bernardo'
  AND edad = 18
  OR edad = 19;
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

* `ALL`: permite actual sobre una lista de valores? (select within select, 6)

### Funciones
Para ayudarnos a la hora de programar existen unas funciones que podemos usar para resolver ciertos problemas:

* `CONCAT()`: concatena cadenas de texto o valores de un campo
```sql
SELECT name
FROM world
WHERE capital LIKE CONCAT(name, ' City');
```

> Muestra las ciudades donde la capital sea igual que el nombre del país mas ' City'

* `REPLACE()`:  sirve para reemplazar caracteres dentro de una cadena. Requiere tres argumentos, el primero será la cadena en la que se va a reemplazar algo, el segundo los caracteres a reemplazar y el tercero los caracteres que se ponen en el lugar de los anteriores

```sql
SELECT REPLACE(name, 'a', '')
FROM world;
```

> Esta consulta mostraría la lista de países eliminando la letra 'a' de todos ellos

* `ROUND()`:  redondea el número que se le pasa como primer parámetro. El segundo parámetro será el número de decimales a mostrar. Si el número es negativo se trunca el resultado en decenas (-1), centenas (-2)...
* `LENGTH()`: devuelve el largo de la cadena que le pasamos como parámetro
* `LEFT()`: devuelve una sub-cadena de la cadena que se le pasa como primer parámetro. El segundo parámetro se usa para indicar el número de posiciones que se cogen de la cadena original
* `AS`: esta función se usa para renombrar un determinado campo, pudiendo usar el nuevo nombre en el resto de la consulta. Se suele utilizar en el `SELECT` para devolver el nombre del atributo que queramos y en el `FROM` cuando se trabaja con varias tablas. La forma de utilizarse es la siguiente:

```sql
SELECT name AS 'Pais'
FROM world AS W;
```

* `DISTINCT`: se emplea en el `SELECT` antes de un atributo para indicar que omita los valores repetidos de este mismo

```sql
SELECT DISTINCT continent
FROM world;
```

> En la tabla "world" cada país tiene su continente asociado, por lo tanto cada continente aparece repetido varias veces en la tabla. Con `DISTINCT` esta consulta sacaría una tabla con cinco tuplas, una por continente

#### Funciones reductoras 

Este tipo de funciones van a reducir el número de tuplas resultado. Habitualmente estas funciones se van a aplicar en sub-tablas, que se generan con un `GROUP BY`. Estas funciones se ponen en el SELECT o en el HAVING del GROUP BY. Todas ignoran los valores **NULL**

* `SUM()`: suma el contenido de las tuplas del atributo que le pasemos por parámetro
* `COUNT()`: cuenta el número de tuplas con valor distinto a NULL del atributo que le pasemos por parámetro
* `AVG()`: hace el promedio del contenido de las tuplas del atributo que pasemos por parámetro
* `MAX()`: recorre las tuplas del atributo que pasemos como parámetro y devuelve una tupla con el valor más alto que encuentre
* `MIN()`: recorre las tuplas del atributo que pasemos como parámetro y devuelve una tupla con el valor más bajo que encuentre

> Cuando usamos una función reductora en el `SELECT` es habitual usar la función `AS` para que en el resultado de la consulta no se vea el nombre de la función.

### Instrucción GROUP BY

Con esta instrucción podemos agrupar en sub-tablas los valores repetidos de un atributo. Por ejemplo, tenemos una tabla con dos atributos, países y continente al que pertenece cada uno. El resultado de agrupar esta tabla por continente sería una sub-tabla por cada uno de los distintos continentes, que tendría los países de cada uno. Una vez agrupados en sub-tablas las funciones reductoras se ejecutarían en cada sub-tabla, pudiendo por ejemplo contar el número de países que hay en cada continente con la función `COUNT()`. 

```sql
SELECT COUNT(name) AS 'Nª paises', continent AS 'Continente'
FROM world
GROUP BY continent;
```

#### HAVING

`HAVING` se usa junto con `GROUP BY` para filtrar las tuplas de cada sub-tabla creada. Según el orden de ejecución de SQL el `WHERE` se ejecuta antes del `GROUP BY` y el `HAVING`, así que las tablas generadas ya estarán filtradas según el predicado del `WHERE`.

```sql
SELECT continent
FROM world
GROUP BY continent HAVING SUM(population) > 100000000;
```

> Selecciona los continentes en los que la suma de su población es mayor que 100 millones
