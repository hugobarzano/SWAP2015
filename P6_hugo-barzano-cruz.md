# Práctica 6. Discos en RAID

- Hugo Bárzano Cruz
- Francisco Javier Garrido Mellado

**Consideraciones Iniciales:** 




## Configuración del RAID por software

**Paso 1:** Intalar software necesario para configurar el raid mediante la intrución 
	sudo apt-get install mdadm
**Paso 2:** Informacion de los discos
	sudo fdisk -l
![imagen] (https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/fdisk.png?raw=true)
sda: disco principal
sdb y sdc disco añadidos para el RAID

**Paso 3:** Crear el RAID 1
	sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc
![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/crear_raid.png?raw=true)
**paso 4:** Dar formato al dispositivo, crear el directorio que vamos a usar y motar la unidad RAID.
	sudo mkfs /dev/md0
	sudo mkdir /datos
	sudo mount /dev/md0 /datos
**Paso 5:** Comprobar el estado del RAID
	sudo mdadm --detail /dev/md0
![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/details.png?raw=true)

**Nota:** Se considera interesante automatizar el proceso de montar el dispositivo RAIZ para que se realice al arrancar el sistema. Para ello es necesario editar el archivo /etc/fstab y añadir:

![imagen](https://github.com/hugobarzano/swap2015/blob/master/capturas_p6/automatizar_montage.png?raw=true) 
	

## Prueba RAID 1

**Paso 1:** Intarlar gestor de arranque y crear un directorio al que llamaremos *archivoPruebaRAID1*
![imagen](raid1_1)
**Paso 2:**  Con la maquina apagada, desconectar el disco sda
![imagen](raid1_2)
    
