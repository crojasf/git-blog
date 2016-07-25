# GIT

Profesor: Miguel Nieva | Twitter: [@mikenieva](https://twitter.com/mikenieva) | [Documentación del curso.](http://git.miguelnieva.com/#/)


## ESTRUCTURA DE ARBOL

1. Working Directory (mi disco duro donde trabajo)
2. Staging área (preparación para subir al repositorio)
3. Repository local (lugar donde se guarda el proyecto, y se sube haciendo "commit")

### Conceptos
* **origin**: Nombre de la conexión con mi Repositorio Remoto de Github.
* **upstream**: Nombre de la conexión remota con el Repositorio principal al cual yo hice Fork en Github.
* **master**: Nombre de la rama principal de cualquier proyecto.
* **rama1**: Se pueden crear nuevas ramas con el nombre que se quiera.
* **origin/master**: Es la rama master “espejo” del Repositorio Remoto descargado en mi Repositorio Local, es un repositorio intermedio entre la rama master del remoto con la rama master de local.
* **upstream/master**: Lo mismo que “origin/master” del Repositorio Remoto Principal.
* **origin/rama2**: Lo mismo que “origin/master” pero para cualquier otra rama de mi Repositorio Remoto.


## VERIFICAR INSTALACION, CONFIG BASICA y HELP

### Mostrar versión instalada
``` php
$ git --version
```

### Configuración básica
``` php
$ git config --global user.name "TU NOMBRE"
$ git config --global user.email "TU CORREO DE GITHUB"
$ git config --global --list
```

### Buscar ayuda de cualquier comando
``` php
$ git help [comando a buscar]
```


## COMANDOS

- git add
- git branch
- git checkout
- git clean
- git clone
- git commit
- git commit -- amend
- git config
- git fetch
- git init
- git log
- git merge
- git pull
- git push
- git rebase
- git reflog
- git remote
- git reset
- git revert
- git status


## HERRAMIENTAS

- git extras
- GitLab
- Zenhub.io


## HEAD

Es el puntero que indica en qué commit y rama que estoy posicionado actualmente, siempre que hago un commit, el HEAD se ubica en ese nuevo commit.


## COMMITS

Todo commit tiene los siguientes datos principales:

* Commit ID (código hexagesimal SHA-1)
* Parent: el commit de donde proviene este commit
* Autor
* Fecha

Todos los commits son SECUENCIALES, no se puede borrar un commit intermedio, solo se puede borrar donde estas en adelante.

### Crear un commit
``` php
$ git commit -m "[Mensaje]"
```

### Crear un commit sin pasar por "add"
``` php
$ git commit -am "[Mensaje]"
```
### Modificar el último commit
En un solo comando se realiza add y commit
``` php
$ git commit -am "[NuevoMensaje]" --amend
```

## CHECKOUT

Sirve para "moverse" entre commits, ramas o tags.
``` php
$ git checkout [SHA commit]
```

Luego de movernos a una parte del proyecto, SIEMPRE hay que regresar donde estábamos trabajando.


## GIT WORKFLOW

### Iteración Básica 
**git add** >> agregamos cambios de Working area a Staging area

**git commit** >> agregamos cambios de Staging area a Repository Local
``` php
// Agrega los cambios al Staging Area
$ git add [file or directory]

// Crea un commit para subir los cambios al Repositorio
$ git commit -m "[Mensaje]"

// Muestra el estado actual del proyecto
$ git status
```

### Workflow básico de ramas de Github
Github utiliza la siguiente estructura de ramas básica para cualquier proyecto (Workflow): 
* **Principal**: es la rama master donde se subirán todos los releases (recomendado tags)
* **Bugs o Hotfixes**: rama para hacer correcciones rápidas directamente a master.
* **Releases**: rama donde se guardan los desarrollos terminados antes de subirlos a master (recomendado tags)
* **Desarrollo**: rama para recopilar todos los desarrollos (features) terminados.
* **Features**: puede ser una o varias ramas utilizadas para cada parte del desarrollo.


## REINICIAL PROYECTO LOCAL

Borrar subcarpeta .git dentro de carpeta del proyecto.


## PASOS BASICOS DE GIT

* Entrar por consola a la carpeta de mi proyecto.

* Iniciar un repositorio en git
``` php
$ git init
```

* Muestra el estado de los archivos del directorio respecto a los archivos rastreados que están listos para hacerles commit.
``` php
$ git status
```

* Subir a Standing Area los ficheros o carpetas con comando "add", parámetro -A es para que suba todo, o si se pone el nombre del fichero hará commit solo de ese fichero
``` php
$ git add -A
$ git add favicon.ico
```

* Subir al repositorio local con comando "commit", se agrega parámetro "-m" y un mensaje entre comillas.
``` php
$ git commit -m "comentario_del_commit"
```

NOTA: se puede hacer “add –A” + “commit –m” con el parámetro “-am”:
``` php
$ git commit -am "comentario_del_commit"
```


## LOG

### Comandos
* **git log --oneline** >> muestra el log una sola línea.
* **git log --decorate** >> indica en que commit está HEAD.
* **git log --stat** >> muestra los cambios a nivel de ficheros que se realizaron.
* **git log --p** >> muestra los cambios que se realizaron.
* **git shortlog** >> muestra los autores y sus commits.
* **git log --graph --online --decorate** >> muestra los asteriscos de las líneas de ramas.
* **git log --pretty=format:"%cn hizo un commit %h el día %cd"** >> Nos permite mostrar mensajes personalizados de los commits.

### Filtros

* **git log -3** >> por cantidad.
* **git log --after="2016-1-2" // git log --after="today" // git log --after="2016-1-1" --before="today"**  >>> por fecha.
* **git log --author="Miguel Nieva"** >> por autor.
* **git log --grep="mensaje" -i** >> por mensaje, el "-i" es para que no sea Case Sensitivity.
* **git log -- index.html** >> por archivo.
* **git log -S"mensaje"** >> por contenido.

### Usos de log

* Para ver el log de los commit, hay parámetros para visualizar mejor los resultados.
``` php
// log estándar.
$ git log
// Si es muy largo el log y aparece END al final, pulsamos la tecla ESC y escribimos “:q” para salir.

// Windows, con "> log.txt" guarda el resultado de "git log" en un fichero llamado log.tx en el directorio actual. 
$ git log > log.txt

// nicelog
git log --oneline --graph --all

// superlog
$ git log --graph --abbrev-commit --decorate --date=relative --format=format:"%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)" --all
```

* Crear Alias de comandos si es que los necesitamos
``` php
// Crear alias
$ git config --global alias.[nombreAlias] "[comando con parametros (qutando git delante)]"
// Borrar alias creado
$ git config --global --unset alias.nombreAlias
// Mostrar contenido de un alias
$ git config --global alias.nombreAlias

/*
NOTA: si el comando lleva comillas dobles, deben cambiarlas por comillas simples porque las dobles deben ir como parámetro del alias.
*/
```


## RESET

Sirve para deshacer los cambios realizados moviéndose entre commits y borrando todo lo creado después del commit al cual queremos ir.

### Reset hard

Hard: Working Directory = Staging Area = Repository 

Borra todos los commits posteriores al SHA indicado. (SHA se obtiene de los logs).

IMPORTANTE: hacer una copia del log antes de ejecutar RESET HARD.
``` php
$ git reset --hard [SHA commit]
```

Se puede inmediatamente después, regresar a uno de los commit borrados, poniendo el SHA de ese commit.


### Reset mixed

Mixed: Staging Area = Repository. Nada con Working Directory. Luego ejecuto Iteración básica (add + commit).

Generalmente se utiliza cuando haces varios commits y luego te das cuenta que puedes unificarlos en uno solo, regresas al anterior y solo queda hacer el commit. 
``` php
$ git reset --mixed [SHA commit]
```

También se puede regresar al último con el SHA del commit borrado.


### Reset soft

Soft: Cambios sólo en el "Repository". Staging = Working. Luego solo commit.

Al igual que el mixed, pero los ficheros modificados ya los deja en Staging Area, solo queda hacer commit.
``` php
$ git reset --soft [SHA commit]
```

También se puede regresar al último con el SHA del commit borrado.


## RAMAS

Una rama es una línea alterna del tiempo en la historia de nuestro repositorio. Por ejemplo: Ideas - Features - Bug Fixes.

La rama principal siempre se llama **master**.

### Crear ramas
``` php
$ git branch [nombreNuevaRama]
$git checkout [nombreNuevaRama]
// o se puede hacer directamente con:
$ git checkout -b "[nombreNuevaRama]"
```

### Mostrar ramas
``` php
$git branch -a
* master
  remotes/origin/master
  remotes/origin/responsivedesign

// con parámetro * se muestra las locales
// con parámetro "remotes" las ramas escondidas de github.
```

El HEAD está en el commit de mi rama **master** y creamos nueva rama, el HEAD se queda posicionado en ese commit de **master** y cuando hacemos un nuevo **commit** desde la nueva rama, recién aparece la nueva rama.

### Cambiar de ramas
``` php
$ git checkout nombreDeRama
```

El HEAD se posiciona en el último commit de dicha rama

### Eliminar ramas
Primero debe estar fusionada con una rama "superior"
``` php
$ git branch -d nombreRamaEliminar
```


## FUSIONES

Hay dos "tipos" de fusiones basándose en conceptos de conflictos:

* **Fast-Forward**: cuando cada rama ataca ficheros diferentes o líneas diferentes dentro de un mismo fichero, no hay conflictos.
* **Manual Merge**: +2 trabajan mismas líneas del mismo fichero, se debe revisar a mano para no generar conflictos.

Nos ubicamos en la rama BASE (la que se mantenga) y ejecutamos el comando con el nombre de la rama PROPUESTA (la que se va a absorber), la línea de tiempo sigue en la rama BASE.

### Fusionar ramas Fast-Forward
``` php
(ramaBase)$ git merge nombreRamaFusionar
```

### Fusion Manual Merge
``` php
(ramaBase)$ git merge nombreRamaFusionar
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Esto sale porque en ambas ramas se estaba editando la misma línea del mismo fichero readme.txt por lo que genera un CONFLICTO.
En el fichero readme.txt se agregan los conflictos, es decir, se agrega lo que se hizo en cada rama
```
<<<<<<< HEAD
Querido mundo
=======
Hola Mundo
>>>>>>> nombreRamaFusionar
```

Se borra manualmente lo que no queremos y guardamos el fichero. Por último realizamos un commit normal y luego las ramas se fusionan.

**NOTA**
Puede salir un mensaje del tipo "Merge remote-traking branch "origin/master", el cual  solicita mensaje, se debe realizar así: Pulsamos tecla "o" para insertar texto, luego pulsamos tecla ESC, luego escribimos ":x" para salvar y pulsamos INTRO.


## REBASE

Reordenar los commits de varias ramas para poner todos los commits en una sola rama.

NOTA: Siempre hacer REBASE y luego MERGE.

Ejemplo: Coger los commits de la rama "ramarebase" y llevarlos al final de la rama "master"
``` php
$ git superlog
* e393272 - (4 seconds ago) 3 - Carlos Rojas (HEAD -> master)
| * 466356d - (2 minutes ago) b - Carlos Rojas (ramarebase)
| * 2ab856b - (2 minutes ago) a - Carlos Rojas
|/
* 5d8cfe6 - (4 minutes ago) 2 - Carlos Rojas
* f113fb6 - (5 minutes ago) 1 - Carlos Rojas
* 448f470 - (6 minutes ago) 0 - Iniciando proyecto 2 - Carlos Rojas

// Hacer Rebase:
(master)$ git checkout ramarebase
(ramarebase)$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: a
Applying: b

(ramarebase)$ git superlog
* 5dc7334 - (5 minutes ago) b - Carlos Rojas (HEAD -> ramarebase)
* 13cb4c5 - (6 minutes ago) a - Carlos Rojas
* e393272 - (4 minutes ago) 3 - Carlos Rojas (master)
* 5d8cfe6 - (8 minutes ago) 2 - Carlos Rojas
* f113fb6 - (9 minutes ago) 1 - Carlos Rojas
* 448f470 - (10 minutes ago) 0 - Iniciando proyecto 2 - Carlos Rojas

// Llevar master al último commit de ramarebase
(ramarebase)$ git checkout master
(master)$ git merge ramarebase

(master)$ git superlog
* 5dc7334 - (9 minutes ago) b - Carlos Rojas (HEAD -> master, ramarebase)
* 13cb4c5 - (9 minutes ago) a - Carlos Rojas
* e393272 - (7 minutes ago) 3 - Carlos Rojas
* 5d8cfe6 - (12 minutes ago) 2 - Carlos Rojas
* f113fb6 - (12 minutes ago) 1 - Carlos Rojas
* 448f470 - (13 minutes ago) 0 - Iniciando proyecto 2 - Carlos Rojas
```


## IGNORE

Para decirle a GIT que ficheros no queremos subir al repositorio remoto de Githhub o al servidor.

* Dentro de la carpeta del proyecto ya iniciado creamos un fichero llamado ".gitignore".

* Dentro de ese fichero, línea a línea agregamos los ficheros o extensiones de que queremos ignorar:
```
*.txt
nombrefichero.php
```
NOTA: los fichero que queremos ignorar NO deben estar subidos al repositorio remoto.


## DIFF (comparar commits)

Sirve para comparar dos commits

* Primero obtenemos los ID de cada commit.
* Nos ubicamos en uno de los commits y ejecutamos:
``` php
$git diff [ID de otro commit]
```

Desde GITHUB se puede comparar directamente

* Entrar a un repo
* Pinchar el enlace de Commits
* Pulsar el ID de un commit para compararlo.


## STASH

Para guardar temporalmente cambios realizados sobre ficheros desde el último commit, en caso que tengas que trabajar en otros cambios diferentes a los que estabas haciendo.

Por ejemplo, estás trabajando en el header de la web pero no has terminado (no has hecho commits) y te piden urgente hacer un cambio en el footer y que hagas un commit solo del footer.
Con STASH guardas temporalmente lo cambiado en header para trabajar lo del footer y luego recuperas lo guardado.

* Ejecutamos el comando donde estemos:
``` php
$ git stash
```
* Trabajamos en el otro cambio y hacemos un commit.
* Listamos los stash creados.
``` php
$ git stash list
```
* Si es el último, aplicamos los cambios a los ficheros.
``` php
$ git stash apply
```
* Los cambios temporalmente guardados se aplican a los ficheros (WIP =Work In PRogress).
* Podemos seguir trabajando y hacer los commits que teníamos planeados.




--------------------------------------------

## URLS INTERESANTES

[Instalar zsh en Windows con “oh my zsh”](http://www.racotecnic.com/2013/12/instalar-zsh-con-oh-my-zsh-y-enlaces-en-menus-contextuales-en-windows-utilizando-cygwin/)

[3.6 Ramificaciones en Git - Reorganizando el trabajo realizado](https://git-scm.com/book/es/v1/Ramificaciones-en-Git-Reorganizando-el-trabajo-realizado)

[Cheat Sheet Git interactivo](http://ndpsoftware.com/git-cheatsheet.html)

[HTML5 Up](https://html5up.net/) - Web para descargar plantillas simples de HTML5

[GITHUB INTEGRATIONS](https://github.com/integrations)
Muestra plugins y conectores a otras herramientas, aplicaciones de Test, etc.

[Pro Git, el libro oficial de Git (librosweb.es)](http://librosweb.es/libro/pro_git/)


Interfaces Web para Bare Repositories Privados:
[Gitweb ](https://git.wiki.kernel.org/index.php/Gitweb)
[gitlist ](https://github.com/klaussilveira/gitlist)
[GitLab](https://about.gitlab.com/downloads/)
Todas las puedes instalar en tu propio servidor yo recomiendo Gitlab tiene una enorme y excelente comunidad



## COMANDOS DE TERMINAL

* Listar directorios
``` php
// Lista los archivos de un determinado directorio
$ ls

// Lista también las propiedades y atributos
$ ls -l

// Lista todos los archivos, incluidos los ocultos y los del sistema
$ ls -a
```

* Navegar directorios
``` php
// ir a una subcarpeta
cd nombreSubCarpeta
cd "Nombre Sub Carpeta"

// subir de nivel
$ cd ..

// ir directo a raiz
cd /

// cambiar de disco duro, x = letra de la unidad
cd /x

// ir directo a carpeta de mi usuario (poner "cd" solo)
cd
// la carpeta de mi usuario de llama "~" (Alt + 126, pero desde Brackets con AltGr)
```
NOTA: Autocompletar: escribimos la primera parte del nombre y luego pulsamos la tecla TAB

* Crear carpeta
``` php
$ mkdir nombreCarpeta
```

* Borrar directorio con ficheros
``` php
$ rm -rf nombreCarpeta
```

* Limpiar pantalla
``` php
$ clear
```

* Editar fichero
``` php
$ vi nombre-readme.txt
// Se sale de edición pulsando ESC y escribiendo “:x”
```

[Más Comandos Linux](http://www.guia-ubuntu.com/index.php/Comandos)

#### FIN