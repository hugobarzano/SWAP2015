# Práctica 6. Discos en RAID

- Hugo Bárzano Cruz
- Francisco Javier Garrido Mellado

**Consideraciones Iniciales:** 




## Configuración del RAID por software

**Paso 1:**Agregar 2 discos duros para montar el RAID e intalar el software necesario para configurar el RAID mediante la instrución
 
	sudo apt-get install mdadm
![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/a%C3%B1adir_discos.png?raw=true)

**Paso 2:** Comprobar que los discos han sido añadidos al sistema y darles formato, mediante los comandos:

	sudo fdisk -l
	fdisk /dev/sdb
	fdisk /dev/sdb

![imagen] (https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/fdisk.png?raw=true)
s
sda: disco principal
sdb y sdc disco añadidos para el RAID

**Paso 3:** Crear el RAID 1
	sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc
![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/crear_raid.png?raw=true)

**paso 4:** Dar formato al dispositivo, crear el directorio que vamos a usar y motar la unidad RAID.
	sudo mkfs.ext4 /dev/md0
	sudo mkdir /datos
	sudo mount /dev/md0 /datos

**Paso 5:** Comprobar el estado del RAID
	sudo mdadm --detail /dev/md0

![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/details.png?raw=true)

**Nota:** Se considera interesante automatizar el proceso de montar el dispositivo RAID para que se realice al arrancar el sistema. Para ello es necesario editar el archivo /etc/fstab y añadir:

	UUID=******* /datos ext4 defaults 0 0
donde ******* es la id del RAID obtenido mediante:

![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/id.png?raw=true)

y dejando el archivo tal que así:

![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/automatizar_montage.png?raw=true)
 
Tras reiniciar, podemos comprobar que el RAID esta funcinando correctamente. Ha pasado de md0 a md127.

![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/reiniciar.png?raw=true)
	

## Prueba RAID 1

**Paso 1:** Conectarse mediante ssh a la maquina virtual y ejecutar el monitor
	watch -n2 cat /proc/mdstat

**Paso 2:** Simular un fallo de disco mediante el comando 
	mdadm --manage --set-faulty /dev/md127 /dev/sdb

![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/raid1_1.png?raw=true)
**Paso 3:** Eliminar el disco duro con el que hemos simulado el fallo mediante el comando:
	mdadm /dev/md127 -r /dev/sdb
![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/raid1_2.png?raw=true)

**Paso 4:** Añadir el disco duro que acabamos de eliminar mediante el comando mdadm /dev/md127 -a /dev/sbd 
![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/raid1_3.png?raw=true)

**Paso 5:** Comprobamos que todo vuelve a la normalidad
![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/raid1_4.png?raw=true)
    
