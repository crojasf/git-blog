# GITHUB

Profesor: Miguel Nieva
Twitter: [@mikenieva](https://twitter.com/mikenieva)
[Documentación del curso.](http://git.miguelnieva.com/#/)


## DEFINICIONES

GitHub es una Plataforma social en la cual desarrolladores suben sus proyectos de código, con el objetivo de mejorarlos a través de la colaboración y gestión.

Es un servicio de “hosting" de repositorios, a través de una interfaz web gráfica.


## PASOS BASICOS EN GITHUB

### Crear repositorio en GitHub

- Entrar a mi usuario de GITHUB.
- Ver Profile.
- Pestaña Repositories.
- Botón New.
- Poner nombre y crear.


### Crear clave SSH en local

Creamos una clave SSH para asociar GitHub con mi Ordenador.

Se crean 2 ficheros, uno privado que irá guardado en una carpeta en local (se recomienda “.ssh”) y el otro se usará para obtener el código de la clave ssh pública y agregarla a Github.

El comando es **ssh-keygen**, y los parámetros a utilizar son:

* -t rsa: es el tipo de codificación.
* -b 4096: es el tamaño de bits.
* -C "[email-de-GitHub]": Es un comentario, pero Github lo usa para asegurar que es tu sitio.
* -f [ruta relativa y nombre-clave]: Por defecto es tu carpeta de usuario (~) pero se puede indicar la ruta relativa a la carpeta .ssh)

Al ejecutarlo nos pide crear contraseña, podemos escribirla o dejarla en blanco, pulsamos INTRO. Confirmamos contraseña anterior o en blanco.

``` php
$ ssh-keygen -t rsa -b 4096 -C "[email-de-GitHub]" -f [ruta-relativa/nombre-clave]
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in .ssh/nombre-clave.
Your public key has been saved in .ssh/nombre-clave.pub.
The key fingerprint is:
SHA256:5OOdmbFrdns………fg4364be5Pk email-de-GitHub
The key's randomart image is:
+---[RSA 4096]----+
|        .#B**.+o |
|   o   . *oO.o+.o|
|      . = *+ o *.|
|     So . .EoO. |
|    . . S   ++.=.|
|       o o . .+ =|
|    .O  . .    = |
|       .:.       o|
|               . |
+----[SHA256]-----+
```

Entramos a la ruta donde guardamos el SSH y ejecutamos comando para mostrar clave SSH
```
$ cat mombre-clave.pub
ssh-rsa kVviMaDTxV35urj0Cc0STDmacjor5w1lKr4JRnQyW7AHSBSOFPxRD1MUuoaceyJWiK9O/F4HJlhLehjqhamTQwTDnT518WOtyd/ySqisVAiMES3aruxiOSWxhWJcykiPXRnIsUtVsv8np4ieShlVrMzGEVLpveU4ZYfx1d78wrPXhv...5jpZNY26hlYs+sR9MTKmLnyajTdqT4Wk2ONWcA8pVK4s3es2SLBoZBrvYbOaqoJM5GY/w== email-de-GitHub
```
El resultado es la Llave Pública, se copia todo el texto "ssh-ra qwert.....12344== email-de-GitHub " y dentro de GitHub la pegamos en Settings > SSH.
Utilizar un nombre descriptivo al equipo, por ejemplo: "Windows PC-CHIQUI"

Repetir el mismo proceso por cada equipo o hosting al cual se vaya a asociar el repositorio.

### Agregar colaboradores

Los colaboradores personas que pueden trabajar directamente con el proyecto (sin necesidad de hacer fork a su cuenta). Según los permisos otrogados podrán hacer commits, push, pull, merge, etc.

En el Repositorio de GitHub, pulsamos la pestaña Settings y en el menú de la izquierda en Collaborators.

## ACCIONES Y CONCEPTOS PRINCIPALES

Dependo si trabajamos con nuestro propio repositorio o con los de otras personas tenemos que tener claro eso:

* **ORIGIN**: es el nombre del "repositorio local espejo" del repositorio remoto en la cual soy propietario, puede ser el repositorio creado por mi o el repositorio creado tras hacer un FORK (copiar el repositorio de otro a mi usuario), o uno creado por la organización en el cual soy colaborador.
* **UPSTREAM*: es el nombre del "repositorio local espejo" del repositorio remoto de otra persona asociado al repositorio forked copiado en mi usuario.

Las acciones principales entre repositorio local (mi PC o el servidor web) y repositorio remoto (Github) son:

* **clone**: Clona repositorio Github en local, copia la carpeta del proyecto donde ejecutemos el comando.
```
$ git clone [repositorio vía SSH ó HTTPS]
```

* **remote**: Realiza una conexión entre remoto y local, no descarga nada. Se ejecuta dentro de la carpeta ya creada con el nombre del proyecto.
```
(master)$ git remote add origin [repositorio propio vía SSH ó HTTPS] 
(master)$ git remote add upstream [repositorio no propio vía SSH ó HTTPS]

```

* **fetch**: Descarga las ramas del repositorio remoto (origin) a un repositorio espejo en local (origin/nombre-de-rama).
```
(master)$ git fetch origin

```

* **merge**: Fusiona la rama indicada del repositorio espejo local al repositorio local.
IMPORTANTE: debo estar ubicado en la rama a la cual quiero “traer” la rama que indico en el comando.
```
(master)$ git merge origin/master
(feature)$ git merge origin/feature
```

* **pull**: Realiza fetch y merge en un solo comando (fetch + merge = pull). Recomendable solo al inicio o si trabajamos solos.
Esto trae de origin (repositorio remoto) la rama master y la guarda en local. Si hay conflictos pedirá hacer una fusión (merge).
NOTA: es preferible hacer "fetch + merge" que "pull" directamente, porque se tiene más control de lo que va haciendo.
```
(master)$ git pull origin master
```

* **push**: Sube los cambios de local a remoto.
Sube la rama master de local a la rama master de origin
```
(master)$ git push origin master
```



## CONEXION CON REPOSITORIO REMOTO

### Conexión Remota
El comando **remote** Sirve para conectar un repositorio remoto con uno local.
```
// Realizar conexión con el repositorio que soy propietario
$ git remote add origin [repositorio vía SSH ó HTTPS]

// Realizar conexión con el repositorio principal del proyecto que hice FORK.
$ git remote add upstream [repositorio vía SSH ó HTTPS]

// Muestra la dirección del remoto de push y fetch
$ git remote -v

// Quitar una conexión remota ya creada
$ git remote remove nombreConexion
```

Es IMPORTANTE que se ejecute el comando desde el directorio local que se va a utilizar para el servidor web. Todos los ficheros contenidos en el repositorio remoto se grabarán aquí.

**Ejemplo práctico Local:**
Si en mi ordenador, la carpeta donde voy a guardar mi web es: "D:\WEBS\www\_crojasf\www\", desde ahí debo ejecutar el comando remote.

**Ejemplo práctico Hosting:**
Si en mi servidor la carpeta donde se ve mi web es: "/public_html/www.crojasf.com", desde ahí debo ejecutar el comando remote.

### Habilitar ssh-agent
En algunos casos es necesario habilitar primero el ssh-agent e indicarle manualmente la ruta de mi clave:
```
// turn on ssh-agent
$ eval $(ssh-agent -s)

// Limpiamos caché de identidades, y agregamos las nuevas llaves:
$ ssh-add -D
All identities removed.

// Agregamos claves de los usuarios:
$ ssh-add ruta_de_clave_rsa/clave_rsa
Identity added: clave_rsa (clave_rsa)
```


## EXPLORACION (Clone) 

El comando **clone** sirve para descargar un proyecto de la web de GITHUB sin intención de colaborar. Crea una carpeta con el proyecto en la ruta donde ejecuto el comando:
``` php
$ git clone [repositorio vía SSH ó HTTPS]
```
Si se quiere utilizar para colaborar (cambiar cosas y subirlas), dentro de la carpeta habrá que hacer un "remote" y luego poder hacer commits, pulls y push.


## COLABORACION (Workflow)

Son flujos de trabajo colaborativos, donde varios desarrolladores trabajan sobre el mismo proyecto.

Hay varios tipos:

### 1) Proyectos creados por ti - Repositorios personales.

- Eres propietario del proyecto. 
- Personas alrededor proponen y tú decides si aceptar o no.
- Proyectos personales, generalmente.

**Iteración básica en tu área local y subir a GitHub**
``` php
$ git init

// Realizamos una iteración básica
$ git commit -am "[Mensaje]"

// Asociarnos en local el repositorio remoto. Solo es necesario hacerlo la primera vez.
$ git remote add origin [SSH]


// ### SUBIR DE LOCAL A GITHUB
// Sube al repositorio remoto "origin" la rama "master"
$ git push origin master

// ### BAJAR DE GITHUB A LOCAL
// Descargamos lo que haya en remoto, se descarga en "origin".
$ git fetch origin

// Fusionamos "origin/master" en "master" del local.
$ git merge origin/master

// "fetch + merge = pull", estos dos pasos se unen en uno solo: "trae origin y cópialo en master".
$ git pull origin master
```

### 2) Proyectos creados por tu organización o equipos.

- Todos son propietarios del proyecto (Colaboradores). 
- Todos suben cambios, sin pedir permisos. 
- Siempre verificar cambios nuevos antes de subir propios.

**Conceptos**

Al descargar un Repositorio Remoto, en el Repositorio Local se crean las ramas duplicadas: una como "origin/master" (rama del repositorio local espejo) y otra como "master" (rama del repositorio local).

"origin/master" es una rama de transición entre la rama master del Remoto con la rama master de Local.

Trabajo en mi rama master de local haciendo commits.

Para unificarlo, debo bajar los posibles cambios del remoto con "fetch", los cambios se bajan en origin/master.

Luego debo fusionar con "merge" de origin/master hacia master de Local, master tendrá todos los cambios (los míos y los nuevos).

Se termina subiendo al Remoto haciendo "push".


**Comandos: Git Fetch + Git Merge + Git Push**
``` php
// Conectamos al repositorio remoto
$ git remote add origin [SSH]

// Descargamos lo que haya en remoto, se descarga en "origin/master".
$ git fetch origin

// Fusionamos "origin/master" en "master" del local.
$ git merge origin/master

// "fetch + merge = pull", estos dos pasos se unen en uno solo: "trae origin y cópialo en master".
$ git pull origin master

** Desarrollamos **

// Descargamos lo que haya en remoto por si hubo cambios.
$ git pull origin master

// Subimos todo al remoto.
$ git push origin master
```

### 3) Proyectos de terceros. (Repositorios “forked”) 

- No son dueños, pero quieren colaborar en el desarrollo.
- Los propietarios aceptan y rechazan propuestas a través de “Pull Request”.

Existirán 2 repositorios: 

a) El repositorio principal, el que no es mío y no soy colaborador.
b) El repositorio forked (La réplica del principal, en tu cuenta de GitHub).

**Conceptos**

Entro en GitHub al proyecto principal logado con mi usuario, y pulso el botón "Fork". Esto clona el repositorio en mi cuenta.

En Local se conectan a ambos Remotos: origin (mi cuenta o forked) y upstream (proyecto principal), y descargamos a local ambos remotos.

En Local quedarían las siguientes ramas: origin/master(forked), upstream/master(principal) y master(local), siendo las dos primeras las de los repositorios locales espejo.

Actualizamos Local siempre del repositorio principal y del forked, antes de comenzar.

Trabajo en mi rama master de Local.

Vuelvo a actualizar del repositorio principal y del forked y realizo "push" que se hace solo a repositorio forked.

Desde GitHub hacemos un "pull request" desde Forked al repositorio principal, pulsamos botón "New pull request", comparamos ramas, luego pulsamos "Create pull request". Dejamos un comentario y pulsamos "Create pull request".

**Comandos**
``` php
// Conectamos a origin(forked) y upstream(principal)
$ git remote add origin [HTTPS ó SSH del proyecto forked]
$ git remote add upstream [HTTPS ó SSH del proyecto principal]

// Bajamos cambios de forked y principal y sincronizamos con local
$ git pull origin master
$ git pull upstream master

//Hacer cambios en local

// Bajamos cambios de principal y sincronizamos con local
$ git pull upstream master

// Subimos cambios a forked
$ git push origin master
```

Luego desde GitHub hacemos un "Pull Request" al repositorio principal.


## PROJECT MANAGEMENT CON GITHUB

### Temario
- Project Management 
- Issues 
- Milestones 
- Tags 
- Usuarios 
- Pull Request en equipos


### Crear Organización

Dentro de GitHub creamos una Organización (el equipo de la empresa), luego agregamos los miembros de la Organización y por último creamos un repositorio.

En el repositorio entramos a Settings > Collaborators y agregamos los colaboradores de ese repositorio, también podemos crear equipos de trabajo.

**Buenas prácticas**: Dentro del repositorio, debajo de las pestañas aparece la descripción, al lado hay un enlace "Edit" y ahí agregamos la URL del proyecto.

Documentar el proyecto con README.md con una estructura básica:
``` php
[Descripción del proyecto]
[Instalación del proyecto en local]
- Requisitos
- Ultima Version
- Encargados del proyecto
[Usage = Uso del proyecto]
- Características
- Plugins
[Documentación]
[Roadmap]
[Licencia]
```

### Issues y Milestones

RESUMEN: Un PROYECTO se divide a primer nivel en MILESTONES, luego cada MILESTONES se divide en ISSUES asignando TAGS según el tipo de ISSUES.

Dentro de la Organización entramos en la pestaña ISSUES para administrar esto.

**Conceptos**

* ** MILESTONES ** es la línea de progreso de etapas del proyecto. Por ejemplo: "desarrollo del blog", "Desarrollo del carrito de compra", "desarrollo del landingpage", etc. Los Milestones son colecciones de Issues y Pull Requests. Se le puede poner fecha de finalización de esta etapa.

* ** ISSUES ** es utilizado para definir trabajos puntuales del tipo ToDos, Bugs, Feature request, etc. Por ejemplo, en un LandingPage habrá: "Header, buscador, sidebar, footer, etc.

* ** TAGS **, son etiquetas para identificar por tipo de trabajo. Por defecto hay: bug, duplicate, enhancement (mejora), help wanted, invalid, question, wonfix.


**Acciones**

Creamos Milestones agregando título, descripción y fecha de fin de desarrollo.

Creamos Issues, uno para cada cosa que necesitamos. Se asocia a un Milestone, se asignan Tags (opcional) y se asigna un desarrollador responsable de ejecutarlo.

Dentro de cada Issue se pueden crear varios comentarios y al final de cierra para indicar que está terminado. Cuando todos los Issues de un Milestone están terminados, de da por concluida esa etapa del proyecto.

### Wiki

Principalmente es para dejar indicado un RoadMap: pasos del proyecto, miembros que hemos agregado, fechas de proyecto, versiones, etc. 

El README.md es más conciso y directo, pero la Wiki es más detallado, ejemplo: [TryGhost](https://github.com/TryGhost/Ghost/wiki)

### Pulse

Es un resumen en tiempo de cómo va yendo el proyecto.

### Graphs

Muestra todos cambios realizados, es como el log de git en gráfico y con más datos.


## GITHUB PAGES

### GitHub Pages para usuarios
Creamos un Repo con el nombre [miUsuarioGithub.github.io] y subimos a la rama master los ficheros.

El contenido se verá en la url: http://miUsuarioGithub.github.io


### GitHub Pages para organizaciones
En un Repo ya existente (nombreRepo), preparamos todos los ficheros en la rama master como siempre.

Cuando todo esté listo para publicar, creamos una rama llamada **gh-pages** y copiamos todo el contenido de mi proyecto en esa rama con push.
```
$ git branch gh-pages
$ git push origin gh-pages
```
El contenido de la rama gh-pages se verá en la url: http://miUsuarioGithub.github.io/nombreRepo


##### FIN