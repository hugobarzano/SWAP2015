# Práctica 3. Balanceo de carga
- Hugo Bárzano Cruz
- Francisco Javier Garrido Mellado

Paso 1: Intalacion de ngid nx
	Para realizar la instalcion del primer software propuesto 
	para funcionar como balanceador de carga es necesaio 		importar la clave del repositorio 
		cd /tmp/
		wget http://nginx.org/keys/nginx_signing.key
		apt-key add /tmp/nginx_signing.key
		rm -f /tmp/nginx_signing.key

	y tras esto, instalarlo mediante apt-get

Paso 2: Configurar nginx
	Para realizar la configuracion de nginx como balanceador de 
	carga, es necesario editar el archivo de configuración
		/etc/nginx/conf.d/default.conf
	y dejarlo tal y como esta en la captura de pantalla