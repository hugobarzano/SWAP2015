# Práctica 6. Discos en RAID

- Hugo Bárzano Cruz
- Francisco Javier Garrido Mellado

**Consideraciones Iniciales:** 




## Configuración del RAID por software

**Paso 1:**Agregar 2 discos duros para montar el RAID e intalar el software necesario para configurar el RAID mediante la instrución
 
	sudo apt-get install mdadm
![imagen](añadir_discos)

**Paso 2:** Comprobar que los discos han sido añadidos al sistema y darles formato, mediante los comandos:

	sudo fdisk -l
	fdisk /dev/sdb
	fdisk /dev/sdb

![imagen] (fdisk)
s
sda: disco principal
sdb y sdc disco añadidos para el RAID

**Paso 3:** Crear el RAID 1
	sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc
![imagen](crear_raid)

**paso 4:** Dar formato al dispositivo, crear el directorio que vamos a usar y motar la unidad RAID.
	sudo mkfs.ext4 /dev/md0
	sudo mkdir /datos
	sudo mount /dev/md0 /datos

**Paso 5:** Comprobar el estado del RAID
	sudo mdadm --detail /dev/md0

![imagen](details)

**Nota:** Se considera interesante automatizar el proceso de montar el dispositivo RAID para que se realice al arrancar el sistema. Para ello es necesario editar el archivo /etc/fstab y añadir:
UUID=******* /datos ext4 defaults 0 0
donde ******* es la id del RAID obtenido mediante:
![imagen](id)
y dejando el archivo tal que así:
![imagen](automatizar_montage) 
Tras reiniciar, podemos comprobar que el RAID esta funcinando correctamente. Ha pasado de md0 a md127.
![imagen](reiniciar)
	

## Prueba RAID 1

**Paso 1:** Me conecto mediante ssh a la maquina virtual y ejecuto el monitor watch -n2 cat /proc/mdstat
![imagen](iniciar_monitor)
**Paso 2:**  Simular un fallo den disco mediante el comando 
	mdadm --manage --set-faulty /dev/md127 /dev/sdb
![imagen](raid1_1)
**Paso 3:** Eliminar el disco duro con el que hemos simulado el fallo mediante el comando:
	mdadm /dev/md127 -r /dev/sdb
![imagen](raid1_2)
**Paso 4:** Añadir el disco duro que acabamos de eliminar mediante el comando mdadm /dev/md127 -a /dev/sbd 
![imagen](raid1_3)
**Paso 5:** Comprobamos que todo vuelve ala normalidad
![imagen](raid1_4)
    
