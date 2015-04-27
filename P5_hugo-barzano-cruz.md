# Práctica 5. Replicación de bases de datos MySQL

- Hugo Bárzano Cruz
- Francisco Javier Garrido Mellado

**Consideraciones Iniciales:** 




## Creación de la base de datos

Acceder a la unterfaz de línea de comandos MySQL:
	*mysql -uroot -p*

Crear la base de datos: 
	*create database contactos;*

Selecionar la base de datos que acabamos de crear: 
	*use contactos;*

La base de datos esta creada pero vacia, vamos a empezar a darle 
contenido, en primer lugar creando una tabla:
	*create table datos(nombre varchar(100),tlf int);*
Ahora vamos a darle contenido a la tabla datos que acabamos de crear:
	*insert into datos(nombre,tlf) values ("hugo",638855122)*
Consultamos que la base de datos ya tiene conenido:
	*show tables;*
	*select * from datos;*
![imagen](https://github.com/hugobarzano/swap2015/blob/master/imagenes/p5/bd.png?raw=true)


