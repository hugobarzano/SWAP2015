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

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/maquina_principal/ab.png?raw=true)

**2- Httperf** 

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/maquina_principal/ab.png?raw=true)

**3- Openload**

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/salidas_p4/maquina_principal/ab.png?raw=true)



