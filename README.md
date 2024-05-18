# Curso de PostgreSQL

## Comandos DQL

- SELECT -> Se utiliza para recuperar datos de una o varias tablas
- FROM -> Especifica la tabla o tablas desde las cuales se deben extraer los datos
- WHERE -> Permite aplicar condiciones para filtrar los datos basándose en criterios de evaluación específicos
- GROUP BY -> Se utiliza para agrupar los datos basándose en una o varias columnas
- HAVING -> Permite aplicar condiciones a los grupos creados mediante GROUP BY
- ORDER BY -> Ordena los resultados en función de una o varias columnas.

 ```
 SELECT * FROM public.personas
 SELECT * FROM public.personas where id = 1 and no_documento = '12345678'
 SELECT * FROM public.personas order by id desc
 SELECT * FROM public.personas order by id asc
 SELECT tipo_documento, COUNT(*) FROM public.personas group by tipo_documento
 ```



## Comandos DML

- INSERT -> Se utiliza para insertar nuevos registros en una tabla.
- UPDATE -> Se utiliza para actualizar datos existentes de una tabla, permite actualizar todos los registros o alguno en específico en función a la clausula WHERE.
- DELETE -> Se utiliza para eliminar una o varias filas de una tabla, obedeciendo a la clausula WHERE, si no está presente, eliminará todos los registros.
- MERGE -> Combina operaciones de tipo INSERT, UPDATE, DELETE en una sola sentencia.

```
INSERT INTO public.personas
(tipo_documento,
 no_documento,
 nombres,
 apellidos,
 fecha_nacimiento,
 correo,
 telefono,
 sexo,
 casado,
 fecha_creacion,
 fecha_actualizacion)
 VALUES
 (
	 'CE',
	 '99885566',
	 'Jack',
	 'Smith',
	 '2000-09-09',
	 'smith.jack@gmail.com',
	 '3048965241',
	 'Masculino',
	 false,
	 '2023-06-15 00:00:00',
	 '2023-06-15 00:00:00'
 );

UPDATE public.personas
SET nombres = 'Jose Daniel'
WHERE id = 1

DELETE FROM public.personas
WHERE id = 2
```

## Comandos DDL

- CREATE -> Crea una nueva tabla, vista, índice o sinónimo.
- ALTER -> Modifica la estructura de una tabla existente, como agregar, mofificar o eliminar columnas.
- DROP -> Elmina una tabla, vista, índice o sinónimo.
- TRUNCATE -> Elimina todos los datos de una tabla, pero conserva la estructura de la tabla.
- RENAME -> Cambia el nombre de una tabla o de una columna existente.

```
-- CREAR UNA TABLA
CREATE TABLE personas(
Id SERIAL PRIMARY KEY,
nombres varchar(20),
apellidos varchar(20),
tipo_documento varchar(20),
no_documento varchar(20),
fecha_nacimiento DATE,
correo varchar(20),
telefono varchar(20),
sexo varchar(20),
casado boolean,
fecha_creacion timestamp,
fecha_actualizacion timestamp
)

--ALTER PARA AÑADIR COLUMNAS
ALTER TABLE public.personas
ADD edad INT

--ALTER PARA CAMBIAR UNA COLUMNA
ALTER TABLE public.personas
ALTER COLUMNA sexo TYPE varchar(20)

--ALTER PARA AÑADIR UN CONSTRAINT
ALTER TABLE public.personas
ADD CONSTRAINT unique_mail UNIQUE (correo)


-- DROP PARA BORRAR LA TABLA
DROP TABLE personas

-- DROP PARA BORRAR LA BASE DE DATOS
DROP DATABASE colegio

--ALTER COMBINADO CON UN DROP
ALTER TABLE public.personas
DROP CONSTRAINT unique_mail

ALTER TABLE public.personas
DROP COLUMN edad

TRUNCATE public.personas

--RENAME
ALTER TABLE public.personas
RENAME TO personas2

ALTER TABLE public.personas2
RENAME COLUMN edad2 TO edad

```

## Comandos DCL

- GRANT -> Otorga privilegios a los usuarios sobre objetos de la base de datos.
- REVOKE -> Revoca los privilegios otorgados anteriormente.

## Comandos TCL

- COMMIT -> Confirma los cambios realizados en una transacción y los guarda de forma permanente en la base de datos
- ROLLBACK -> Descarta todos los cambios realizados en una transacción y los devuelve al estado anterior a la transacción
- SAVEPOINT ->  Crea un punto de guardado dentro de una transacción, lo que permite realizar un ROLLBACK parcial hasta ese punto, sin deshacer toda la operación
- RELEASE SAVEPOINT -> Elimina un punto de guardado creado previamente con SAVEPOIN. Después de haber ejecutado este comando no se podrá realizar un ROLLBACK parcial hasta ese punto.

```
  BEGIN;


INSERT INTO public.personas2
(tipo_documento,
 no_documento,
 nombres,
 apellidos,
 fecha_nacimiento,
 correo,
 telefono,
 sexo,
 casado,
 fecha_creacion,
 fecha_actualizacion)
 VALUES
 (
	 'CE',
	 '99885566',
	 'Jack',
	 'Smith',
	 '2000-09-09',
	 'smith.jack5@gmail.com',
	 '3048965241',
	 'Masculino',
	 false,
	 '2023-06-15 00:00:00',
	 '2023-06-15 00:00:00'
 );
  
 SELECT * FROM public.personas2;
 
ROLLBACK; --TIRAR PARA ATRAS LOS CAMBIOS
COMMIT; --CONFIRMAR LOS CAMBIOS

```

# Tipos de Datos en PostgreSQL

- INTEGER -> Entero de 32 bits
- BIGINT -> Entero de 64 bits
- DECIMAL -> Numero decimal de precisión variable
- JSON -> Es un tipo de dato usado especialmente en postgresaql, para almacenar datos de formato JSON de forma eficiente en la base de datos.
- SERIAL -> En realidad no es un tipo, es un alias, que genera una secuencia de forma automática, su tipo real es un entero (INT) , pero se usa en postgresql para declarar que un campo es de valor autoincrementable.
- BOOLEAN -> Valor booleano (verdadero o falso)
- TEXT -> Cadena de texto de longitud variable
- VARCHAR -> Cadena de texto variable con longitud máxima especificada
- DATE -> FECHA
- TIMESTAMP -> FECHA Y HORA
- GEOMETRY -> Datos de geometria (puntos, líneas, polígonos etc)
- BYTEA -> Datos binarios de longitud variable

# Tipos de relaciones en bases de datos (SQL) Aplica para bases de datos ISO/IEC 9075:2016 o SQL:2016

- Relación uno a uno (one-to-one) -> En esta relación, un registro en una tabla se relaciona con exactamente un solo registro en otra tabla y viceversa, Se utiliza cuando dos entidades tienen una relación directa y exclusiva entre sí.
 Ejemplo: Tabla A (Cliente) -> id, nombre, dirección
          Tabla B (Cliente_Detalle) -> id, id_cliente, fecha_nacimiento, numero_telefono, correo

  ```
      --EJERCICIO RELACION 1-1 Y RELACION 1-N
    --SCRIPT PARA CREAR TABLA ESTUDIANTES
    create table estudiantes(
    Id SERIAL primary key,
    nombres varchar(50),
    apellidos varchar(50),
    correo varchar(100)
    );
    --SCRIPT PARA CREAR TABLA DIRECCIONES CON RELACION 1-1
    create table direcciones(
    id serial primary key,
    nombre_direccion varchar(50),
    direccion varchar(100),
    ciudad varchar(50),
    pais varchar(50),
    id_estudiante integer references estudiantes(id) on delete cascade unique);
    -- SCRIPT PARA CREAR TABLA DIRECCION CON RELACION 1-N
    create table direcciones(
    id serial primary key,
    nombre_direccion varchar(50),
    direccion varchar(100),
    ciudad varchar(50),
    pais varchar(50),
    id_estudiante integer references estudiantes(id) on delete cascade);
    insert into public.estudiantes (nombres, apellidos, correo) values ('Pedro', 'Suarez', 'pedro.suarez@gmail.com');
    select * from public.estudiantes
    select * from public.direcciones d
    insert into public.direcciones (nombre_direccion, direccion, ciudad, pais, id_estudiante) values ('Casa', 'cra busquela con calle encuentrela', 'armenia', 'colombia', 1)
    select e.id , e.nombres , d.direccion from estudiantes e inner join direcciones d on d.id_estudiante = e.id


  ```
      
- Uno a muchos (one-to-many) -> En esta relación, un registro en una tabla se relaciona con varios registros en otra tabla, pero cada registro en la segunda tabla, solo puede relacionarse con un único registro en la primera tabla. Se utiliza cuando una entidad se asocia con múltiples instancias de otra entidad.
  Ejemplo: Departamento -> id, nombre
  Empleado -> id, nombre, id_departamento
  En este caso, cada departamento de la tabla A, puede tener varios empleados relacionados en la tabla B, pero cada empleado solo puede pertenecer a un departamento.
 
- Relacion de muchos a muchos (many-to-many) -> En esta relación varios registros de una tabla, pueden relacionarse con varios registros en otra tabla y viceversa, Se utiliza cuando varias entidades se relaciones entre sí de forma no exclusiva, para poder lograr este tipo de relaciones, necesitamos una tabla intermedia.
 Ejemplo: Tabla A (Estudiante) -> id, nombre
          Tabla B (Curso) -> id, nombre, duracion
          Tabla C tabla intermedia (Matriculas) -> id, id_estudiante, id_curso
 ```
*RELACION M-M cursos-estudiantes

create table cursos(
id serial primary key,
nombre varchar(50),
codigo_curso int,
duracion int
);

*TABLA INTERMEDIA
create table matriculas(
id serial,
id_estudiante integer references estudiantes(id),
id_curso integer references cursos(id),
primary KEY(id_estudiante, id_curso) 
);

*llave compuesta, llave primaria compuesta, me garantiza que los campos seleccionados dentro de la llave compuesta, sean únicos en todos los registros de esta tabla.


insert into public.estudiantes (nombres, apellidos, correo)
values
('Daniel', 'Garces', 'm-7@gmail.com'),
('Santiago', 'Jimenez', 'm-6@gmail.com'),
('Albeiro', 'Salazar', 'm-4@gmail.com'),
('Matias', 'Campo', 'm-2@gmail.com'),
('Daniela', 'Martinez', 'm.2@gmail.com');

insert into cursos (nombre, codigo_curso , duracion) values ('Filosofia', 78,1),('Redes 1', 45,2);

select * from public.cursos c;
select * from public.estudiantes e;

insert into matriculas (id_estudiante, id_curso) values (3,1), (3,2), (3,3)


select e.id , e.nombres , e.apellidos , c.nombre , c.duracion  from estudiantes e 
inner join matriculas m on m.id_estudiante = e.id 
inner join cursos c on c.id = m.id_curso 
where e.id = 3

 ```
## Ejemplo de constraints on delete

```
create table clientes(
id SERIAL primary key,
nombre varchar(50)
);

-- ON DELETE CASCADE , si la tabla principal elimina un registro, todos los registros en la tabla secundaria seran eliminados
create table pedidos(
id SERIAL primary key,
cliente_id integer, 
descripcion varchar(100),
foreign key (cliente_id) references clientes(id) on delete cascade 
);

-- ON DELETE SET NULL
create table pedidos(
id SERIAL primary key,
cliente_id integer, 
descripcion varchar(100),
foreign key (cliente_id) references clientes(id) on delete set null 
);

-- ON DELETE RESTRICT
create table pedidos(
id SERIAL primary key,
cliente_id integer, 
descripcion varchar(100),
foreign key (cliente_id) references clientes(id) on delete restrict
);


drop table public.clientes cascade
drop table public.pedidos 

insert into clientes(nombre) values ('Pedro'), ('Juan'), ('Alberto');

insert into pedidos (cliente_id, descripcion) values
(1, 'Pedido de ejemplo 1'),
(1, 'Pedido de ejemplo 2'),
(1, 'Pedido de ejemplo 3'),
(2, 'Pedido de ejemplo 1'),
(2, 'Pedido de ejemplo 2'),
(2, 'Pedido de ejemplo 3');

select * from public.clientes

select * from public.pedidos

delete from public.pedidos where cliente_id = 1

delete from public.clientes where id = 1
```
  
