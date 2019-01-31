# Preparación de entorno
### Primeros pasos Ubuntu 18

1. Pre-requisitos 
2. Instalación [webmin](http://www.webmin.com/)
3. Tomcat 8.5

## Pre-requisitos

- Nano
- Openjdk

### Nano

Abrimos un terminal y nos conectamos por **ssh** al servidor

	ssh root@h2701747.stratoserver.net

Vamos a instalar el editor de texto **nano**

	sudo apt-get install nano

### Openjdk
Instalamos el openjdk

	sudo apt-get install openjdk-8-jre

Comprobamos la versión

	java -version

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

## Tomcat 8.5

### Instalación tomcat
 Procedemos a instalar el servidor tomcat 8.5
 
	sudo apt install tomcat8  

Para iniciar/parar o comprobar el estado de tomcat

	sudo service tomcat8 start
	sudo service tomcat8 stop	
	sudo service tomcat8 status
	
Con el servidor levantado si entramos en la siguiente ip nos aparecera la bienvenida de tomcat

	https://your_server_ip:8080

### [Plugin](https://github.com/AcuGIS/Tomcat-Webmin-Module) tomcat para webmin

Nos dirigimos a:
		
	-> webmin configuration
	-> Módulos de webmin
	-> Install from ... ** Desde dirección URL ftp o http **
	-> https://cdn.acugis.com/apache-tomcat-webmin-plugin/tomcat.wbm.gz
		
Una vez instalado nos vamos a:

	-> Servidores
	-> Apache Tomcat
Si todo fue **ok** debería estar funcionando perfectamente.
