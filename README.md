# Preparación de entorno
### Primeros pasos Ubuntu 18

1. Pre-requisitos 
2. Instalación [webmin](http://www.webmin.com/)

## Pre-requisitos

Abrimos un terminal y nos conectamos por **ssh** al servidor

	ssh root@h2701747.stratoserver.net

Vamos a instalar el editor de texto **nano**

	sudo apt-get install nano
	
## Instalación webmin

Editamos el siguiente archivo con nuestro editor de texto

	sudo nano /etc/apt/sources.list

Agregamos el siguiente repositorio al final del archivo para que ubuntu tenga acceso a instalador

	deb http://download.webmin.com/download/repository sarge contrib
	
Guardamos el fichero **ctrl + o** y salimos **ctrl + x**

A continuación, agreguegamos la clave PGP de Webmin para que el sistema confíe en el nuevo repositorio:

	wget http://www.webmin.com/jcameron-key.asc
	sudo apt-key add jcameron-key.asc
	sudo apt update

Instalamos webmin
	
	sudo apt install webmin

Si la instalación ha ido correcta podemos acceder a nuestro panel

	https://your_server_ip:10000 

