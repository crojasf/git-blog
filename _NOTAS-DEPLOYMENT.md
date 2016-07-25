# HOSTING - DEPLOYMENT

Profesor: Miguel Nieva
Twitter: [@mikenieva](https://twitter.com/mikenieva)
[Documentación del curso.](http://git.miguelnieva.com/#/)


## HOSTING PROPIO

Tenemos que solicitar al hosting un acceso a SSH para acceder a los ficheros de las webs instaladas en nuestro hosting.

Se debe generar una clave SSH en el servidor y la pública ponerla en GITHUB, al igual que en LOCAL (ver notas de GITHUB).

En mi hosting la conexión es con clave privada y por puerto 93:
```
$ ssh -i ruta-de-mi-clave/nombre-clave miusuario@midominio.com -p 93
The authenticity of host '[midominio.com]:93 ([000.000.000.00]:93)' can't be established.
RSA key fingerprint is SHA256:qwerty…………..123456.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[midominio.com]:93,[ 000.000.000.00]:93' (RSA) to the list of known hosts.
Enter passphrase for key nombre-clave.ppk': <<<< AQUI PONEMOS LA PASS DE ACCESO AL HOSTING
Attempting to create directory /home/ miusuario /perl5
miusuario @ midominio.com [~]#
```
Es la única conexión que otorgan por lo que es la misma para todos los servicios que tengo.

En el hosting debo tener instalado GIT, y por consola se podría instalar
```
// En mi hosting ya está instalado, si no lo está:
$ yum install git
```

Se debe traer el repositorio de Github al servidor ejecutando el comando **desde la carpeta donde se va a mostrar la web**, por ejemplo:
```
$ cd /public_html/www.midominio.com/
$ git init
$ git remote add origin [HTTPS ó SSH del proyecto]
$ git pull origin master
```
NOTA: tener mucho cuidado donde ejecutamos los comandos para no estropear otras partes del hosting.


## AMAZON WEB SERVICES - AWS

### Introducción

Es un servicio de Hosting escalable de forma automática y con muchas opciones de configuración.

Es gratuito si el tráfico es bajo, pero de pago en función de la necesidad puntual del tráfico.

### Acciones básicas para montar un Hosting.

Entramos a [Amazon Web Services](https://aws.amazon.com/es/) y nos logamos con nuestra cuenta de Amazon. En "Mi cuenta" entramos a "Consola de administración de AWS".

Arriba a la izquierda entramos en **EC2** para empezar la configuración del site.

**Key Pair**: creamos una clave dando un nombre y automáticamente se descarga un fichero "nombrekey.pem". Lo guardamos en nuestro ordenador en la carpeta ".ssh". Se recomienda aplicar seguridad al fichero desde Terminal dando permisos de lectura solo a mi usuario:
```
$ chmod 0400 fichero.pem
```

**Security Groups**: creamos reglas de seguridad para permitir entradas a puerto 80 de html y 22 para SSH. Por ejemplo: nombre "load-balancer-NOMBRE", descripción "Allow 80 22", agregamos regla HTTP y SSH.

**Load Balancer**: creamos lógicas de balanceo, por ejemplo, si detecta mucho tráfico, parte de él lo deriva a otra instancia con una copia idéntica de mi proyecto.

**Instancia** Es una sesión de un Sistema Operativo en el Servidor donde poder subir mi proyecto, como una máquina virtual. Aquí irán los ficheros de mi web. Dejamos generalmente todo por defecto, solo necesitaremos darle un nombre y seleccionar un Security Group.

### Conectarse a una instancia de AWS desde Terminal por SSH

Conectarse por SSH a la instancia con este comando de terminal:
```php
$ ssh -i ficherollave.pem ipPublicaInstancia -l usuarioAWS
```
Ejemplo: 
```php
$ ssh -i ejemplo.pem 00.00.000.00 -l ec2-user

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-ami/2016.03-release-notes/
[ec2-user@ip-000-00-00-000 ~]$
```

### Primeras instalaciones y acciones en la instancia

Aquí instalaremos LAMP, GIT y daremos permisos de sudo a nuestro usuario.

Actualizamos yum
```shell
$ sudo yum update -y
```

Instalamos AMP: Apache, MySQL y PHP
```shell
$ sudo yum install -y httpd24 php56 mysql55-server php56-mysqlnd
```

Inicialamos Apache
```shell
$ sudo service httpd start
```

Configuramos Apache para que inicie en cada inicio de sistema
```shell
$ sudo chkconfig httpd on
```

Instalar GIT:
```shell
$ sudo yum install git
```

Dar permisos de sudo al usuario ec2-user a la carpeta www navegando hasta ella
``` shell
$ cd /var/www
```

Creamos grupo llamado "www".
``` shell
$ sudo groupadd www
```

Agregamos usuario ec2-user al grupo www.
``` shell
$ sudo usermod -a -G www ec2-user
```
OJO: Salimos del sistema y volvemos a entrar para que se apliquen los permisos.

Verificamos permisos de grupo.
``` shell
$ groups
ec2-user wheel www
```

Cambiamos de "dueño" del de carpeta /var/www.
``` shell
$ sudo chown -R root:www /var/www
```

Cambiamos permisos en carpeta y subcarpetas, otorgando los de escritura.
``` shell
$ sudo chmod 2775 /var/www
$ find /var/www -type d -exec sudo chmod 2775 {} \;
```

Cambiamos lo permisos a los fichero de las carpetas.
``` shell
$ find /var/www -type f -exec sudo chmod 0664 {} \;
```
NOTA: Si no hacemos esto, tendremos que poner "sudo" delante de casi todos los comandos que ejecutemos.

### Traer el proyecto desde Github

Navegar hasta donde está la carpeta html de la instancia:
``` shell
$ cd /var/www/html/
```

Iniciar GIT:
``` shell
$ git init
```

Traer el repositorio:
``` shell
$ git remote add origin [SSH]
$ git pull origin master
```

En el navegador entramos a la URL de la Web: http://DNS.Servicio/nombre-repositorio-git/ 

A partir de aquí ya podemos hacer los PULL que queramos desde GIT



##### FIN
