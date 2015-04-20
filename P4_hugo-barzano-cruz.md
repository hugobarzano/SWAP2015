# Práctica 4. Comprobar el rendimiento de servidores web

- Hugo Bárzano Cruz
- Francisco Javier Garrido Mellado

**Consideraciones Iniciales:** 

**Ip-servidor principal (máquina individual): 172.16.24.128**

**Ip-Balanceador de carga (granja web): 172.16.24.130**

El software que he utilizado para medir el rendimiento del servidor web ha sido:

**1- Apache Benchmark** Desde la máquina anfitrión mediante el comando:

	ab -n 100000 -c 100 http://172.16.24.130/

**2- Httperf** Desde la máquina anfitrión mediante el comando:

	httperf --server 172.16.24.130 --port 80 --uri /index.php --rate 150 --num-conn 27000 --num-call 1 --timeout 5

**3- Openload** Desde la máquina anfitrión mediante el comando:

	openload 172.16.24.130 100


## Rendimiento servidor web principal de manera individual 
 

**1- Apache Benchmark** 

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/maquina_principal/ab.png?raw=true)

**2- Httperf**

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/maquina_principal/httperf.png?raw=true)	

**3- Openload**

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/maquina_principal/openload.png?raw=true)	


## Rendimiento granja web con balanceo de carga nginx

**1- Apache Benchmark** 

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/balanceador_nginx/ab.png?raw=true)

**2- Httperf** 

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/balanceador_nginx/httperf.png?raw=true)

**3- Openload**

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/balanceador_nginx/openload.png?raw=true)

## Rendimiento granja web con balanceo de carga ha-proxy

**1- Apache Benchmark** 

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/balanceador_haproxy/ab.png?raw=true)

**2- Httperf** 

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/balanceador_haproxy/httperf.png?raw=true)

**3- Openload**

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/balanceador_haproxy/openload.png?raw=true)

## Conclusiones herramienta Apache Benchmark 
![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/media_ab.png?raw=true)

Fijandonos en los valores medios obtenidos por AB podemos obserbar que la maquína individual tarda mas en realizar los test pero tambien obtiene un menor numero de fallos por cada petición.
Podemos observar tambien que el servidor balanceado, obtiene un mejor tiempo por cada peticion a si como un mejor tiempo en las peticiones a las que da servicio en cada segundo.
## Conclusiones herramienta  Httperf
![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/media_httperf.png?raw=true)

Si utilziamos la herramienta Httperf podemos obserbar que ante el mismo número de conecsiones, el servidor, de manera individual, produce menos respuestas y un mayor numero de errores que si estamos utilziando el balanceador de carga Nginx. Los resultados obtenidos en este caso para Ha-proxy son despreciables ya que el software de balanceo no esta funcionando correctamente. 
## Conclusiones herramienta Openload    
![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/media_openload.png?raw=true)

Por último, con la herramienta Openload  podemos observar que nginx es esl que mas trassaciones completa por segundo, seguido muy de cerca por Ha-proxy. Tambien podmeos observar que la maquina individual es la que mayor tiempo medio de respuesta produce.

## Conclusión Final
En vista de los resultados obtenidos por 3 herramientas distintas, para 3 maneras distintas de configurar nuestro servidor, podemos concluir que en mi caso con una maquina anfitrión Intel(R) Core(TM) i5 CPU M 460 @ 2.53GHz / 4GB Ram, el software balanceador Nginx es el que mejor resultados da para el rendimiento de la granja web. Hay que tener en cuenta que para realizar este esperimento, he utilziado el script.php que nos facilitó el profesor, para incrementar la carga de trabajo en cada petición. Si realizaramos el esperimento con una pagina web estática obtendriamos resultados muy similares a los realizados por mi compañero de prácticas ![url](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica4/P4_Francisco-Javier-Garrido-Mellado.md) en los que se obtiene que el mayor rendimiento se encuentra en la máquina de manera individual.   


