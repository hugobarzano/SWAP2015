#PRACTICA 2: Clonar la información de un sitio web

- Hugo Bárzano Cruz
- Francisco Javier Garrido Mellado

**1-** *Generar la clave ssh mediante el comando:*

	ssh-keygen -t dsa

**2-** *Copiarla al servidor principal mediante el comando:*

	ssh-copy-id -i .ssh/id_dsa.pub root@172.16.24.128

	y efectivamente, tras esto, podemos conectarnos via ssh sin necesidad de introducir contraseña.
	![imagen] (https://github.com/hugobarzano/swap2015/blob/master/imagenes/P2/ssh_copy.png?raw=true)

**3-** *Automatizar la tarea de clonar la informacion de nuestro sitio web:*

       Para ello, editar el fichero /etc/crontab y añadir la siguiente linea:
	**01 *   ***  root rsync -avz -e ssh root@172.16.24.128:/var/www/ /var/www/**
 
	la cual ejecutará la herramienta rsync en el primer minuto de cada hora, de todos los dias de la semana, de todos los meses.
