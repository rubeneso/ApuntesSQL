# DML - Manipulación de datos
**DML** (Data Manipulation Language) es el sub-lenguaje de SQL que agrupa los comandos necesarios para introducir, modificar y eliminar datos en una tabla de SQL
>**¡OJO!** Esta sección está documentada para el gestor de bases de datos **PostgreSQL**, las instrucciones y su sintaxis pueden ser diferentes en otro gestor de bases de datos

### Índice
 - [Instrucción INSERT](#instrucci%c3%b3n-insert)
 - [Instrucción UPDATE](#instrucci%c3%b3n-update)
 - [Instrucción DELETE](#instrucci%c3%b3n-delete)

## Instrucción INSERT
Con esta instrucción insertaremos filas de datos en una tabla. Tendremos que indicar a que tabla se le insertan los valores, los nombres de los atributos a los que asignaremos valores y los valores que se insertan. El orden de los atributos y de los valores debe ser el mismo. La sintaxis es la siguiente:
```sql
INSERT INTO <nombre_tabla> [(<atributo1>, <atributo2>, ...)] 
(VALUES (<valor1>, <valor2>, ...) | SELECT ...);
```
> Si se omiten los atributos a los que se asignan los valores se utiliza el orden de creación de los atributos para el orden de los valores

> Si en vez de especificar los valores estos se sacan de otra tabla con un `SELECT`, la consulta deberá devolver el número de columnas adecuado y en el orden correcto

Ejemplo:
```sql
INSERT INTO alumnos (dni, nombre, clase) 
VALUES ('12345678A', 'Alicia', '2º B');
```
Se pueden añadir varias filas con una sola instrucción de la siguiente manera:
```sql
INSERT INTO alumnos (dni, nombre, clase) VALUES 
    ('12345678A', 'Alicia', '2º B'),
    ('12345678B', 'Bernardo', '2º A'),
    ('12345678C', 'Celia', '1º A');
```
Ejemplo de `INSERT` sacando los valores de otra tabla:
```sql
INSERT INTO alumnos (dni, nombre, clase) 
SELECT dni, nombre, clase
FROM nuevosAlumnos;
```
[Volver al índice](#%c3%8dndice)
## Instrucción UPDATE
Con esta instrucción podemos modificar los datos de una tabla. Se puede modificar solo una fila de datos, todas las filas o solo las especificadas. Tenemos que indicar que tabla y que atributo o atributos se van a modificar, el nuevo valor y que filas se modifican. Esta es la sintaxis:
```sql
UPDATE <nombre_tabla> 
SET <nombre_atributo1> = <nuevo_valor1>, <nombre_atributo2> = <nuevo_valor2>, ... 
[WHERE <condicion_para_escoger_filas>];
```
> Si se omite el `WHERE` se estarían escogiendo todas las filas de la tabla

Ejemplo:
```sql
UPDATE productos 
SET cantidad = 0
WHERE nombreProducto = 'Papel Higiénico';
```
> Con esta operación seleccionamos la fila de la tabla productos que corresponde al papel higienico e indicamos que se ha acabado el stock

También podemos referirnos al anterior valor del atributo para establecer su nuevo valor. En la siguiente operación se establece que el precio del producto 'Arroz' sea el mismo que tenía antes multiplicado por 1,20:
```sql
UPDATE productos 
SET precio = precio * 1.20
WHERE nombreProducto = 'Arroz';
```
> Hay que tener mucho ojo con la condición del `WHERE`, ya que si no la ponemos o ponemos una condición incorrecta, podríamos sobreescribir todas las filas de la tabla con el mismo valor

[Volver al índice](#%c3%8dndice)
## Instrucción DELETE
Con esta instrucción se pueden eliminar filas de una tabla. Se debe especificar la tabla y una condición con un `WHERE` donde se especifique que filas se van a eliminar. **Si se omite el WHERE se eliminarán todas las filas**:
```sql
DELETE FROM <nombre_tabla> [WHERE <condicion_para_escoger_filas>];
```
Ejemplo:
```sql
DELETE FROM productos WHERE numeroVentasMes < 5;
```
> Eliminamos de la tabla los productos de los que se hayan vendido menos de 5 unidades en un mes

[Volver al índice](#%c3%8dndice)