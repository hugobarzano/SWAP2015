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

## Replicar la base de datos con mysqldump (manualmente)
	**Servidor Principal -> copia de seguridad**
	Paso 1: Bloquear acceso a la BD 
		mysql -u root -p
		FLUSH TABLES WITH READ LOCK;
		quit
	Paso 2: Utilizar mysqldump para guardar datos
		mysqldunmp contactos -u root -p > /root/copiaBD.sql
	Paso 3: Desbloquear las tablas
		mysql -u root -p
		UNLOCK TABLES;
		quit
	**Servidor Respaldo -> Restaurar copia de seguridad**
	Paso 1: Copiar el archivo copiaBD.sql del servidor principal
		scp root@172.16.24.128:/root/copiaBD.sql /root/
	Paso 2: Crear la base de datos en el servidor de respaldo:
		mysql -u root -p
		create database BDrespaldo;
		quit
	Paso 3:Restaurar  los datos contenidos en la BD del 		servidor principal
		mysql -u root -p BDrespaldo < /root/copiaBD.sql
Podemos observar como la base de datos ha sido replicada correctamente en el servidor de respaldo:
![imagen](https://raw.githubusercontent.com/hugobarzano/swap2015/87612ebda74d5d5a1014422d0f59b0dabe35a233/imagenes/p5/bd_respaldo.png)

## Replicar la base de datos mediante configuración maestro-esclavo
	**Servidor Principal**
	Paso 1: Comentar el parametro bind-address
		#bind-address 127.0.0.1
	Paso 2: Almacenar el log de errores
		log_error = /var/log/mysql/error.log
	Paso 3: Establecer identificador del servicio
		server-id = 1
	Paso 4: Almacenar el registro binario
		log_bin = /var/log/mysql/bin.log
	Paso 5: Guardar el documento y reiniciar servicio. Podemos comprobar que la configuración es correcta, ya que no se 		han producido errores.

	![imagen](https://raw.githubusercontent.com/hugobarzano/swap2015/87612ebda74d5d5a1014422d0f59b0dabe35a233/imagenes/p5/configuracion_maestro.png)

	**Servidor Respaldo**
	Paso 1: Establecer identificado del servicio
		server-id=1
	Paso 2: Establecer datos master(solo en mysql inferior 5.5)
		Master-host= 192.168.45.140
		Master-user=usuariobd
		Master-password=123456
	En mi caso no he tenido que realizar este paso. Reinicio el 
	servicio y compruebo que no se han producido errores:
	
	![imagen](https://github.com/hugobarzano/swap2015/blob/master/imagenes/p5/configuracion_esclavo.png?raw=true)

	**Creación de usuario para replicación**
	Nos situamos en la maquina principal y creamos un usuario 
	com permisos de replicación.
	Paso 1: CREATE USER esclavo IDENTIFIED BY 'esclavo';
	Paso 2: GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%' IDENTIFIED BY 'esclavo';
	Paso 3: FLUSH PRIVILEGES;
	Paso 4: FLUSH TABLES;
	Paso 5: FLUSH TABLES WITH READ LOCK; 
	Paso 6: Obtener los datos de la BD a replicar SHOW MASTER STATUS;
	![imagen](https://github.com/hugobarzano/swap2015/blob/master/imagenes/p5/master_status.png?raw=true)

	Paso 7: En la maquina esclava, le damos los datos del maestro:
	CHANGE MASTER TO MASTER_HOST='172.16.24.128',
	MASTER_USER='esclavo', MASTER_PASSWORD='esclavo',
	MASTER_LOG_FILE='mysql-bin.00001', MASTER_LOG_POS=501,
	MASTER_PORT=3306;

	Paso 8: Arrancar el esclavo
	START SLAVE;
	Paso 9: Desbloquear las tablas del maestro
	UNLOCK TABLES;
	Paso 10: Comprobar que funciona
	SHOW SLAVE STATUS\G 
	![imagen] (https://github.com/hugobarzano/swap2015/blob/d6440572095f8615951bf5090992cca807d2783c/imagenes/p5/slave_status.png?raw=true)
	Copia las nuevas inserciones:
	![imagen](https://github.com/hugobarzano/swap2015/blob/d6440572095f8615951bf5090992cca807d2783c/imagenes/p5/salida_final.png?raw=true)







