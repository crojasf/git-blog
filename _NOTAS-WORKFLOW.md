# WORKFLOW EN EQUIPO

Profesor: Miguel Nieva
Twitter: [@mikenieva](https://twitter.com/mikenieva)
[Documentación del curso.](http://git.miguelnieva.com/#/)


## INTRODUCCION

Se realizará un ejercicio en el cual se crearán dos usuarios en Github y en GIT de local se configurarán ambos en la misma máquina, luego se harán Push y Pull Requests al Repositorio en Github, y por último se subirá el proyecto (Deploy) a AWS.


## CREAR CLAVES SSH

Creamos una clave SSH para cada usuario, primero las cuentas reales de github
```
$ cd ~/.ssh

$ ssh-keygen -t rsa -b 4096 -C "usuario1@dominio.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/.ssh/id_rsa): github- usuario1_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in github- usuario1-rsa.
Your public key has been saved in github- usuario1-rsa.pub.
The key fingerprint is:
SHA256:+XCJAAApa//NzzVsdsdsdsdsdsdsdsdsdAZCilcE usuario1@dominio.com
The key's randomart image is:
+---[RSA 4096]----+
|**o+.            |
|= ….           |
|++.   .  .       |
|=+.    ..o..  o  |
|=o..    S+o  E   |
|o...  . .++ o .  |
|.  .. o. o.B +   |
|    .o..o O o    |
|   . o. .= .     |
+----[SHA256]-----+
```

Lo mismo con el segundo usuario, la clave la llamamos github-usuario1_rsa
```
$ ssh-keygen -t rsa -b 4096 -C "usuario2@dominio.com"
.....
```

## ASOCIAR CLAVES A CUENTAS GITHUB

Dentro de Github agregamos para cada usuario sus respectivas llaves públicas para que ambas cuentas estén asociadas al mismo ordenador.

## MULTIPLE USUARIO EN GIT LOCAL

Dentro de la carpeta donde se guardan las claves (.ssh por ejemplo) creamos un fichero llamado "config" (sin extensión) y dentro escribimos esto:

```
# github Cuenta1
Host cuenta1
   HostName github.com
   User git
   IdentityFile ~/.ssh/claveCuenta1

# github Cuenta2
Host cuenta2
   HostName github.com
   User git
   IdentityFile ~/.ssh/claveCuenta2
```

Si estamos utilizando el Bash de Git en Windows, hay que habilitar ssh-agent:
```
$ eval $(ssh-agent -s)
$ ssh-add -D
All identities removed.
$ ssh-add claveCuenta1
Identity added: claveCuenta1 (claveCuenta1)
$ ssh-add claveCuenta2
Identity added: claveCuenta2 (claveCuenta2)
```

Hacemos test de que funcionen las llaves
```
$ ssh -T cuenta1
$ ssh -T cuenta2
```
NOTA: Si pregunta "Are you sure you want to continue connecting (yes/no)?" escribimos 'yes', solo lo hará una vez.


Creamos un directorio para cada usuario.
```
cd ...../carpeta_proyecto/
$ mkdir dirCuenta1
$ mkdir dirCuenta2
```

## AGREGAR REPOSITORIOS A CADA DIRECTORIO
Agregamos los repositorios remotos en cada usuario y cada carpeta:

### Usuario1
En teoría es nuestro usuario se siempre, por lo que no hay que configurar nada de nombre ni mail.
Entramos a carpeta de cuenta1
```
$ cd ...../dirCuenta1
```
Iniciamos git
```
$ git init
```
Conectamos a remoto:
IMPORTANTE: Traemos el repo remoto, **pero en el SSH reemplazamos** "github.com" por "cuenta1".
```
$ git remote add origin git@cuenta1:nombre-repositorio.git
```
Descargamos el repositorio remoto:
```
$ git pull origin master
```

## Usuario 2
En teoría es un usuario que hayamos creado de prueba para practicar, por lo que será la primera vez que utilicemos.

Repetimos pasos de entrar a carpeta, iniciar git y de realizar la conexión de Cuenta1 pero con Cuenta2
```
$ cd ...../dirCuenta2
$ git init
$ git remote add origin git@cuenta2:nombre-repositorio.git
```
Antes de descargar algo, debemos configuramos el nombre y mail de la nueva cuenta2 (solo se hace una vez)
```
$ git config user.name "Nombre Cuenta2"
$ git config user.email "mailCuenta2@cuenta2.com"
```
Descargamos el repositorio remoto:
```
$ git pull origin master
```

Ahora para el ejercicio solo basta con cambiar de carpeta en carpeta y automáticamente se cambia de usuario.

## GIT FLOW 

Básicamente consiste en tener disciplina de trabajo y seguir la regla de oro: 

**"ANTES DE HACER CUALQUIER COSA... HACER UN FETCH PARA BAJAR LO PENDIENTE"**

Quiere decir, que antes de empezar a trabajar o de subir algún cambio realizado, debemos bajar todos lo posibles cambios realizados por algún compañero.


## EJEMPLO DE FLOW CON RAMAS

### Cuando los usuarios son colaboradores

Básicamente es que, cuando hago fetch, me avisa que existen nuevas ramas para descargarlas primero.

Luego entro a esa nueva rama descargada con "git checkout nombreRamaNueva" y hago un merge en local

El trabajo de cada rama es independiente uno de otro, por ejemplo, la hacer push debo hacer uno en cada rama:
```
$ git checkout master
(master)$ git fetch origin
(master)$
// Si no sale nada es que no hay nada pendiente, por lo que puedo hacer push tranquilamente
(master)$ git push origin master

// Luego hago lo mismo con la otra rama:
(master)$ git checkout nuevaRama
(nuevaRama)$ git fetch origin
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 3), reused 5 (delta 2), pack-reused 0
Unpacking objects: 100% (6/6), done.
From crojasf-test:nombre-de-repositorio
   3eae2e9..68de2a5  master     -> origin/master
   5443f31..ecafd2f  responsivedesign -> origin/responsivedesign

// Indica que hubo cambios previos, entonces debo hacer un merge
(responsivedesign)$ git merge origin/responsivedesign
// Si todo OK, ahora sí puedo hacer push de lo que he estado trabajando.
```


### Cuando mi usuarios no es colaborador

Desde Github se debe hacer un FORK del repositorio principal a mi repositorio.

Luego ese repositorio forked bajarlo a local.

Recordar que ahora crea 2 repositorios espejos local ocultos: origin(mi fork) y upstream(principal), por lo que ahora las ramas estarán triplicadas: local, origin y upstream.

Se hace igual que si yo fuera colaborador pero solo puedo hacer push al repositorio forked, y luego de Github se hace un pull request al repositorio principal.

#### Consola
Iniciamos
```
$ git init
```

Conexión origin (repo forked) y a upstream (repo principal)
```
(master)$ git remote add origin git@github.com:micuenta/nombre-repositorio.git
(master)$ git remote add upstream git@github.com:cuenta-dueño-repo-principal/nombre-repositorio.git
```

Bajamos upstream y merge con master (pull lo hace en un paso)
```
(master)$ git pull upstream master
```

Trabajamos y realizamos una iteración básica (add + commit)

Buscamos cambios en repo principal (upstream)
```
(master)$ git fetch upstream
```

Si no hay cambios OK, si hay cambios debo fusionar con master
```
(master)$ git merge upstream/master
```

Ahora ya puedo hacer push pero a mi origin (no puedo hacerlo a upstream)
```
(master)$ git push origin master
```

#### Github

Con usuario2 (el colaborador) entramos al repositorio Forked y pulsamos botón "Pull request".

Vemos que ramas vamos a comparar y pulsamos "Create pull request"

Ponemos título y descripción, y pulsamos "create pull request"

Con usuario1 (administrador) entramos al repositorio principal y en la pestaña "Pull request" aparecerán todos los pedidos.

Entramos al Pull request solicitado y podemos rechazarlo o pulsar "Merge pull request" para fusionar esos cambios.


##### FIN
