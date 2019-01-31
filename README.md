# Preparación de entorno
### Primeros pasos Ubuntu 18

1. Pre-requisitos 
2. Instalación [webmin](http://www.webmin.com/)
3. Ftp
4. Tomcat 8.5
5. MariaDB
6. Nginx

## Pre-requisitos

- Nano
- Nuevo usuario root (opcional)
- Openjdk
- Software-Properties-Common

### Nano

Abrimos un terminal y nos conectamos por **ssh** al servidor

	ssh root@h2701747.stratoserver.net

Vamos a instalar el editor de texto **nano**

	sudo apt-get install nano

### Nuevo usuario root

**Crear usuario**

Ejecutamos , nos pedirá  el password a crear

	sudo adduser nombre_usuario

Editamos el fichero para dar permisos sudo
		
	sudo /usr/sbin/visudo
		
Escribimos en la linea debajo de root

	root            ALL=(ALL:ALL) ALL 
	nombre_usuario	ALL=(ALL:ALL) ALL

**Cambiar pass**

	sudo passwd nombreusuario

**Eliminar usuario**

	sudo userdel nombreusuario
	
		

### Openjdk
Instalamos el openjdk

	sudo apt-get install openjdk-8-jre

Comprobamos la versión

	java -version

### Software-properties-common	

Para agregar repositorios fácilmente con un comando (en este caso sirve en el paso de MariaDb)

	sudo apt-get install software-properties-common

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
	
## Ftp

1.  En el buscador busca FTP.
2.  Selecciona: ProFTPD
3.  Haga clic en Instalar módulo en el caso de que no lo tengas instalado.
4.  En la barra lateral izquierda, después de la instalación, haga clic en  **Refresh Modules**
5.  Haga clic en Crear un nuevo usuario, que se encuentra en Sistema -> Usuarios y grupos
6.  Proporcionar un  **nombre de usuario para lo que debe ser su cuenta de FTP**.
7.  Seleccione Contraseña normal y proporcione una contraseña única y compleja para la cuenta.
8.  Si quieres puedes escoger un directorio o carpeta personal personalizado.
9.  Seleccione  **Nuevo grupo con el mismo nombre**  que el usuario y dale a  **Crear**.

Una vez hecho esto, deberías poder acceder desde FTP con el nombre de usuario y la contraseña que pusistes, utilizando la dirección IP de su servidor como host.  
Recuerda que si has cambiado de puerto SFTP tendrás que poner el nuevo puerto.

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

### [Modulo](https://github.com/AcuGIS/Tomcat-Webmin-Module) tomcat para webmin

Nos dirigimos a:
		
	-> webmin configuration
	-> Módulos de webmin
	-> Install from ... ** Desde dirección URL ftp o http **
	-> https://cdn.acugis.com/apache-tomcat-webmin-plugin/tomcat.wbm.gz
		
Una vez instalado nos vamos a:

	-> Servidores
	-> Apache Tomcat
	
Si todo fue **ok** debería estar funcionando perfectamente.

## MariaDb
Podemos instalar MariaDb desde los repositorios de ubuntu o los propios de MariaDb, recomiendo esto último para obtener la ultima versión.

#### Repositorio ubuntu
Para instalar desde el repositorio de ubuntu escribimos

	sudo apt update
	sudo apt install mariadb-server

#### Repositorio MariaDB
Agregamos la clave de confianza

	sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8

Agregamos el repositorio

	sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://ftp.utexas.edu/mariadb/repo/10.3/ubuntu bionic main'

Si recibe un mensaje de error que dice **add-apt-repository command not found** instalaremos **software-properties-common** (ver pre-requisitos)

Actualizar e instalar

	sudo apt update
	sudo apt install mariadb-server

#### Comandos de verificación

Verificación de inicio

	sudo systemctl status mariadb

Versión

	mysql -V

### Ajustes webmin

El módulo de webmin ya viene preinstalado pero posiblemente tengamos que cambiar el path del fichero de configuración ya que por defecto coge el de Mysql y no el de MariaDb

Nos dirigimos a 

	-> Un-usedmodules
	-> Mysql Database Server
Si nos aparece un cartel como el siguiente, es que tenemos que cambiar la ruta

	The MySQL config file /etc/mysql/mariadb.cnfs was not found on your system. Use the module configuration page to set the correct path.

Para ello

	-> Pulsamos la rueda dentada
	-> System configuration
	-> MySQL configuration file
	
	Cambiamos el path por:
	
	/etc/mysql/mariadb.cnf

### Configuración segura

Por defecto la bbdd solo escucha en localhost, hay que abrirla al exterior:

	-> MySQL Server Configuration
	-> MySQL server listening address 
	-> Any

Ejecutamos el  comando `mysql_secure_installation` para mejorar la seguridad de la instalación de MariaDB donde nos pedirá contraseña nueva de root:

	sudo mysql_secure_installation

Para entrar desde cualquier dominio el host debe ser

	%
Otros comandos utiles


	SELECT * FROM mysql.user
  
	CREATE USER 'nombre_usuario'@'ip_o_host' IDENTIFIED BY 'mi_clave';

	//comando de solo select insert delete
	GRANT SELECT ON * . * TO 'nombre_usuario'@'%';
	GRANT INSERT ON * . * TO 'nombre_usuario'@'%';
	GRANT DELETE ON * . * TO 'nombre_usuario'@'%';
	
## Nginx

### Instalación del servidor

Me he basado en el siguiente [link](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04#step-5-setting-up-server-blocks-(recommended))

Instalamos

	sudo apt update
	sudo apt install nginx

Comprobamos el servicio

	systemctl status nginx

Accedemos a la ip de nuestro servidor para comprobar el mensaje de bienvenida

	http://85.214.22.45/

Comandos de interes

	sudo systemctl stop nginx
	sudo systemctl start nginx
	sudo systemctl restart nginx
	sudo systemctl reload nginx
	sudo systemctl disable nginx
	sudo systemctl enable nginx
	
### [Modulo](https://www.justindhoffman.com/project/nginx-webmin-module) nginx para webmin

Nos dirigimos a:
		
	-> webmin configuration
	-> Módulos de webmin
	-> Install from ... ** Desde dirección URL ftp o http **
	-> https://www.justindhoffman.com/sites/justindhoffman.com/files/nginx-0.11.wbm_.gz

Una vez instalado nos vamos a:

	-> Servidores
	-> Nginx webserver

### Host virtuales

Vamos a crear varios host virtuales para redirigir el trafico a la pagina que corresponda en función del dominio o subdominio que ha hecho la petición web:

	sudo mkdir -p /var/www/nombre_dominio.com/html	
	sudo chown -R $USER:$USER /var/www/nombre_dominio.com/html
	sudo chmod -R 755 /var/www/nombre_dominio.com
	
Editamos el fichero para agregar la configuración

	sudo nano /etc/nginx/sites-available/nombre_dominio.com

Agregamos la siguiente configuración cambiando por nuestro nombre de dominio

	server {
	        listen 80;
	        listen [::]:80;

	        root /var/www/nombre_dominio.com/html;
	        index index.html index.htm;

	        server_name nombre_dominio.com www.nombre_dominio.com;

	        location / {
	                try_files $uri $uri/ =404;
	        }
	}

Agregamos un enlace simbolico

	sudo ln -s /etc/nginx/sites-available/nombre_dominio.com /etc/nginx/sites-enabled/

Y reiniciamos nginx
	
	sudo systemctl restart nginx
