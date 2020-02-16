# DQL - Consultas SQL
**DQL** (Data Query Language) es el sub-lenguaje de SQL que agrupa los comandos necesarios para realizar cualquier tipo de consulta a una base de datos, para obtener los datos que podamos necesitar en un determinado momento.
### Índice
- [DQL - Consultas SQL](#dql---consultas-sql)
    - [Índice](#%c3%8dndice)
  - [Sentencias y filtros](#sentencias-y-filtros)
    - [Sentencia SELECT](#sentencia-select)
    - [Instrucción WHERE](#instrucci%c3%b3n-where)
    - [Funciones](#funciones)
      - [Funciones reductoras](#funciones-reductoras)
    - [Instrucción GROUP BY](#instrucci%c3%b3n-group-by)
      - [HAVING](#having)
    - [Instrucción ORDER BY](#instrucci%c3%b3n-order-by)
    - [Sub-consultas o consultas anidadas](#sub-consultas-o-consultas-anidadas)
      - [Sub-consultas sincronizadas](#sub-consultas-sincronizadas)
    - [Acceso a varias tablas - JOIN](#acceso-a-varias-tablas---join)
      - [Self-JOIN](#self-join)
    - [Valores nulos](#valores-nulos)
## Sentencias y filtros
### Sentencia SELECT
La sentencia de consulta de SQL es `SELECT`.
Se usa para seleccionar los campos de los que queramos recuperar la información. Después de ella debe ir la sentencia `FROM`, que indica la tabla de la que sacamos los valores. Un ejemplo sería el siguiente:

```sql
SELECT nombre, dni
FROM alumnos;
```
Es importante recalcar que todas las consultas en SQL se terminan con un punto y coma, es una regla del lenguaje. Los campos a recuperar se escriben uno a uno separados por comas. Para recuperar todos los campos de la tabla usamos el asterisco "*".

### Instrucción WHERE

Si solo queremos recuperar las tuplas(filas) que cumplan una determinada condición usamos `WHERE`. Seguido del `WHERE` se pone un predicado, que se define como una función que devuelve "true" o "false". Esta instrucción se va a recorrer todas las tuplas de la tabla y solo devolverá aquellas tuplas que cumplan con las condiciones del predicado. Por ejemplo, si quisiéramos buscar a todos los alumnos que se llamaran Bernardo, escribiríamos:
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
  AND (edad = 18
  OR edad = 19);
```



Operadores para las condiciones: 

* Igual que: = | LIKE
* Menor que: <
* Menor o igual que: <=
* Mayor que: >
* Mayor o igual que: >=
* Distinto: <> | IS NOT
* Nulo: null | IS NULL

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

### Instrucción ORDER BY

Como el nombre de la misma indica, se usa para ordenar una consulta según uno o varios campos, pudiendo hacerse de manera ascendiente y descendiente. La forma de usarla es la siguiente:
```sql
SELECT continent, name
FROM world
ORDER BY continent DESC;
```
> Esta consulta saca una lista de los continentes y países, ordenada de manera descendente por el campo "continent"
```sql
SELECT continent, name
FROM world
ORDER BY continent DESC, name ASC;
```
> Y esta otra saca una lista de los continentes y países ordenados primero por continente en orden descendiente y después ordenados por país en orden ascendente

### Sub-consultas o consultas anidadas
Las consultas anidadas son consultas que están dentro de otras consultas. Estas se suelen poner en el `WHERE` para comparar atributos. La sintaxis es la misma que una consulta normal, pero la sub-consulta deberá ir dentro de paréntesis "()" y no se puede poner inmediatamente después del `WHERE`, sino que tendrá que ponerse primero el atributo a comparar, después el operador y después la sub-consulta. Otra cuestión importante sobre las sub-consultas es que están en un ámbito distinto a la consulta exterior. Por ejemplo, las tablas de la consulta exterior y de la interior no son las mismas, si se quisiera llamar a la tabla exterior habría que usar un alias.
```sql
SELECT name
FROM world
WHERE population > (
  SELECT population
  FROM world
  WHERE name = 'Russia'
);
```
> Esta consulta sacaría todos los países cuya población es mayor que la población de Rusia
#### Sub-consultas sincronizadas
Este tipo de sub-consultas funciona como un bucle anidado y hacen referencia a la fila actual de su consulta externa. En el `WHERE` de la sub-consulta se compara el mismo atributo pero de la tabla interior y de la exterior. Para hacer esto se deben usar alias distintos para la tabla de fuera y de dentro.
```sql
SELECT continent, name, population 
FROM world AS w1
WHERE population >= ALL(
  SELECT population 
  FROM world AS w2
  WHERE w1.continent = w2.continent
);
```
> Esta consulta sacaría continente, país y población del país con más población por cada contiente

> Podemos leer el `WHERE` interior como "donde los valores correlacionados son iguales"
### Acceso a varias tablas - JOIN
La sentencia `JOIN` permite acceder a más de una tabla a la vez en la misma consulta. Para ello se debe añadir `JOIN` después de la tabla que ya tengamos el el `FROM`, y también añadir un `ON` para indicar sobre que atributos se debe combinar la tabla. Si esto no se indica de ninguna manera, obtendríamos el producto cartesiano de las dos tablas, es decir, que por cada tupla de la primera tabla tendríamos todas las tuplas de la segunda. El `ON` es habitual que esté compuesto por la clave principal de una tabla y la clave ajena de la otra. En caso de que la relación de las tablas sea de N:M existirá una tabla intermedia formada por las claves ajenas de las dos tablas. Este es un ejemplo suponiendo que un alumno solo tiene un profesor, y un profesor puede tener varios alumnos:
```sql
SELECT alumno.nombre
FROM profesor JOIN alumno ON profesor.id = alumno.profeId
WHERE profesor.nombre = 'Alicia';
```
> Esta consulta saca una lista de los alumnos a los que Alicia imparte clase. Vease que se necesita hacer el `JOIN` ya que necesitamos el nombre del alumno y el nombre del profesor, que están en diferentes tablas

> Cuando se utiliza más de una tabla es recomendable que se ponga precediendo a cada atributo el nombre de la tabla a la que pertenece, para evitar confusiones y ver la consulta más clara

Esta sería otra consulta pero esta vez usando tablas que tienen una relación N:M:
```sql
SELECT pelicula.titulo, actor.nombre
FROM pelicula 
  JOIN casting ON pelicula.id = casting.peliculaId
  JOIN actor   ON casting.actorId = actor.id;
```
> Esta consulta sacaría una lista de todos los actores y todas las peliculas que han hecho

> Se usa la tabla casting porque es la que contiene las claves ajenas de las dos tablas y es la que se tiene que usar para poder relacionarlas y usarlas en la consulta

**Apunte:** el predicado del `ON` puede moverse a un `WHERE` y la consulta seguiría funcionando igualmente
#### Self-JOIN

### Valores nulos
*TODO
