# PostgreSQL
## Crear una base de datos
`Create database <nombre>`

## Tipos de datos

- boolean = true or false
- character(n) = cadena de caracteres de n tamaño, FIJO
- date = fecha sin hora
- Float = flotante
- Int, integer = numero entero
- decimal = numero exacto
- time = tiempo en horas, minutos, segundos, etc
- varchar(n) = cadena de caracteres de tamaño variable

## BASICO

**Eliminar una base de datos (Tiene que estar desconectada)**
`Drop database if exists "<nombre>"`

```sql
create table persona(
	idpersona int not null, este parametro jamas sera null
	nombre varchar(20), -- Tamano del char
	cedula varchar(10)
);
```

**Insertar valores**

```sql
insert into <tabla> values('1', 'Erik', '34353')

insert into persona values('1', 'Erik', '34353')
-- Especificar valores pocisionalmente
insert into persona (idpersona, nombre) values('1', 'Erik')
```

**Hacer una consulta**

```sql
-- traer todo desde <tabla>
select * from <tabla>

-- traer las columnas idpersona, nombre
select idpersona, nombre from persona
```

**Modificar registros**

```sql
-- De la tabla persona cambia id persona a 2
update persona set idpersona = '2'
-- donde cedula es null
where cedula is null

update persona set nombre = 'Ivanov'
where cedula = '6543'

-- Actualizar varias columnas
update persona set nombre = 'Citlali', cedula = '8555'
where idpersona = 5
```

## select

**Traer todo desde <tabla>**
`select * from <tabla>`

**Traer columnas especificas en paréntesis
`select (col1, col2) from <tabla>`

**Traer columnas y renombraras**
__POSTGRESQL NO ES SENSIBLE A MINUSCULAS Y MAYUSCULAS__
`select col1 as nombre, col2 from <tabla>`

**Traer con condición**

```sql
select nombre from persona
where idpersona = '5'
```

## where y condicionales

**Condicionales**

> =
> !=
>
> <
> <=
> =
> Operadores
> and
> or

**Borrar TODOS registros**

```sql
-- Borrar todos los registros
delete from <tabla>
```

**Borrar con una condición**

```sql
-- Borrar todos los registros
delete from <tabla>
where <condicion>
-- Ejemplo:
delete from persona
where idpersona = 3
```

## alter

**Agregar columna nueva**

```sql
alter table <tabla>
add column <nombre_de_la_columna_nueva> <tipo>

-- Ejemplo
alter table persona
add column test varchar(20)
```

**Renombrar tabla**

```sql
alter table <tabla>
rename column <nombre_actual> to <nuevo_nombre>

-- Ejemplo
alter table persona
rename column test to testo
```

**eliminar columna**

```sql
alter table <tabla>
drop column <nombre_de_la_columna>

-- Ejemplo
alter table persona
drop column testo
```

**Clave primaria**

Sirve para que una columna tenga un valor único

```sql
create table personas(
	idpersona int not null,
	nombre varchar(20),
	cedula varchar(10),
	primary key (idpersona) 
)
```

**Agregar primary key a una columna existente**

```sql
alter table <table>
add primary key(<columna>)

-- Ejemplo
alter table persona
add primary key(idpersona)
```

## serial

Los valores de un campo serial, se inician en 1 y se incrementan en 1 automáticamente.

**Crea una tabla con id que autoincrementa en 1 **

```sql
create table test(
	idtest serial primary key not null,
	dato int,
	descripcion varchar(20)
)
```

## drop

**Eliminar una tabla**

`drop table <nombre_tabla>`

## truncate

**Reiniciar la tabla y serial**

`truncate table <nombre_tabla> restart identity`

## Valores DEFAULT

Crear una tabla con valores por default

```sql
create table test(
	idtest serial primary key not null,
	dato int,
	descripcion varchar(20) default 'Nada.'
)
```

## Cálculos en columnas

**Hacer calculo cuando se traen datos**

```sql
select <col1>, <colINT> + <numero> as <nombre> from <tabla> 

-- Ejemplo 
select nombre, (sueldo + 600) as sueldo from trabajdor
```

**Hacer un update con una operación**

```sql
update <tabla> set <colINT> = <colINT> + <algo>

-- Ejemplo
update trabajador set sueldo = sueldo + 5
```

****Hacer un update con una operación y condición**

```sql
update <tabla> set <colINT> = <colINT> + <algo>
where condicion

-- Ejemplo
update trabajador set sueldo = sueldo + 100
where idtest = 2
```

## Ordenar

```sql
select * from <tabla> order by <dato>

-- Ejemplo
-- A - Z
select * from persona order by nombre
-- Z - A
select * from persona order by nombre desc
```

## Buscar Registros

```sql
select * from <tabla>
-- Buscar texto que tenga e con texto a la izquierda y derecha
where dato like '%e%' 
```

## Contar registros

**Contar todos los registros**

```sql
select count(*) from <tabla>
```

**Contar registros con condición**

```sql
select count(*) from <tabla>
where <condicion>
```

## Funciones con Registros

**sum()**: Suma números de columnas 

```sql
-- Suma col1
select sum(col1) from <tabla>
```

**max()**: da el máximo valor

```sql
select max(col1) from <tabla>
```

**min()**: da el mínimo valor

```sql
select min(col1) from <tabla>
```

**Agrupación con otra columna**

Esto sirve cuando hay datos repetidos

```sql
select <col1>, min(<col2>) from <tabla>
group by <col1>
```

**avr()**: Da el promedio

```sql
select avg(col1) from <tabla>
```

## distinct

Es para filtrar los resultados para que no haya valores repetidos

 ```sql
select distinct <col> from <tabla>
 ```

## between

es para asignar un rango

```sql
-- Buscar datos que esten en el rango 20 a 50
select * from <tabla>
where <col> between 20 and 50

-- Ejemplo
select * from trabajador
where sueldo between 500 and 700
```

Para fuera de ese rango 

```sql
select * from trabajador
where sueldo not between 500 and 700
```

## Restringir valores

Hacer que le valor sea único en una columna

```sql
alter table <tabla>
add constraint uq_<col>
unique(<col>)
```

Quitar restricción

```sql
alter table <tabla>
drop constraint uq_<col>
```

 ## Llave foránea

Crear una columna que llame primary keys de otra tabla

```sql
-- Agregar columna donde se va a almacenar la refrenecia
alter table publicaciones
add idusuariopub int

-- Agrgar llave foranea para que haga refrencia
alter table publicaciones
-- una restriccion
add constraint fk_id
-- idusuariopub sera la clave foranea
foreign key(idusuariopub)
-- Que hace referencia a <tabla>(<col>)
references usuarios(idusu)
```

## Crear funciones

```sql
create or replace function <mombre>(parametros...)
returns <tipo>
as
$$
{Cuerpo de la funcion}
$$
language sql


-- Ejemplo
create or replace function suma(n1 int, n2 int)
returns int
as
$$
select n1 + n2
$$
language sql

-- Usar
select suma(2, 5)

-- Ejemplo 2 que no retirna nada
create function insertarUsuarios() 
returns void as
$$
-- LEVAN ;
insert into usuarios(nombre, descripcion) values('A', 'Hola');
insert into usuarios(nombre, descripcion) values('B', 'Hola1');
insert into usuarios(nombre, descripcion) values('C', 'Hola2');
insert into usuarios(nombre, descripcion) values('D', 'Hola3');

$$
language sql 
-- usar
select insertarUsuarios()

-- Retorna una tabla
create or replace function buscarporid(int)
returns usuarios as
$$
select * from usuarios
where idusu = $1 
$$
language sql
```

