# Práctica 4. Comprobar el rendimiento de servidores web

- Hugo Bárzano Cruz
- Francisco Javier Garrido Mellado

**Consideraciones Iniciales:** 

**Ip-servidor principal (máquina individual): 172.16.24.128**

**Ip-Balanceador de carga (granja web): 172.16.24.130**

El software que he utilizado para medir el rendimiento del servidor web ha sido:

**1- Apache Benchmark** Desde la maquina anfitrion mediante el comando:

	ab -n 100000 -c 100 http://172.16.24.130/

**2- Httperf** Desde la máquina anfitrión mediante el comando:

	httperf --server 172.16.24.130 --port 80 --uri /index.php --rate 150 --num-conn 27000 --num-call 1 --timeout 5

**3- Openload** Desde la máquina anfitrión mediante el comando:

	openload 172.16.24.130 100


## Rendimiento servidor web principal de manera individual 
 

**1- Apache Benchmark** 

	

**2-** *Configurar nginx*

	Para realizar la configuracion de nginx como balanceador de 
	carga, es necesario editar el archivo de configuración
		/etc/nginx/conf.d/default.conf
	y dejarlo tal y como se muestra en la siguiente captura:

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/imagenes/P3/nginx_default_conf.png?raw=true)

	es importante reiniciar el servicio despues de realizar cualquier modificacion en la configuración
	ya que si no los cambios no se realizarán. Utilizar:  
			
		sudo service nginx restart

**3-** *Comprobar que el software funciona*

	Mediante el comando curl probamos que nginx esta balanceando la carga correctamente. Es aconsejable
	comentar la tarea cron que configuramos en la practica 2 ya que si no, ambos servidores (principal y respaldo)
	tendrian el mismo index.html y no sabriamos distingir si el balanceo se esta realizando correctamente o no.

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/imagenes/P3/nginx_balancea.png?raw=true)

## Balanceo de carga con haproxy

**Consideraciones Iniciales:** Podemos utilizar una cuarta maquina virtual para realizar la
instalación de este software pero en el caso de que queramos hacerla sobre la maquina 3, donde
acabamos de instalar y configurar nginx como balanceador, debemos parar dicho servicio e iniciar el
nuevo. Podemos hacerlo con: 

	sudo service nginx stop					*detener nginx*
	sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg	*Iniciar haproxy*

Si queremos volver a balancear con nginx:
	
	sudo killall haproxy					*detener haproxy*
	sudo service nginx start				*iniciar nginx*

   
**1-** *Instalación de haproxy*

	Mediante el comando:

		sudo apt-get install haproxy

**2-** *Configuración de haproxy*

	Para configurar haproxy como balanceador de carga es necesario editar el fichero de configuración /etc/haproxy/haproxy.cfg
	dejandolo tal y como se muestra en la siguiente captura:

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/imagenes/P3/haproxy_conf.png?raw=true)

**3-** *Comprobar que el software funciona*

	Mediante el comando curl probamos que haproxy esta balanceando la carga correctamente

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/imagenes/P3/ha_proxy_balancea.png?raw=true)










