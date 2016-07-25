# OTROS CONCEPTOS AVANZADOS

Profesor: Miguel Nieva
Twitter: [@mikenieva](https://twitter.com/mikenieva)
[Documentación del curso.](http://git.miguelnieva.com/#/)


## BARE REPOSITORIES

Son repositorios simples y propios que no tiene Working Directory ni Staging Area, solo el repo con las ramas.

Generalmente se instala en una carpeta del servidor y ahí se guarda el proyecto GIT, desde local se sube ahí todo con Push. Por último se clona desde este Bare Repo a una carpeta del servidor los ficheros del propio proyecto web, esto se llama "desplegar el proyecto" o ·"Deployment".

A los Bare Repos básicamente solo se puede hacer Push, Pull y Clone.

Github realmente es un Bare Repo, solo que con la interface visual muestra todo el histórico del proyecto.

**Comandos**

En el server creamos bare repo
```
$ git init --bare nombreRepo.git
```

En local clonamos del server
```
$ git clone SSH_de_servidor_BareRepo:ruta_de_BareRepo/nombreRepo.git nombreRepoClonada
```
NOTA  la ruta del Bare Repo es relativa a la carpeta en la que la conexión te posiciona al conectarte, p.e., Hostsuar te posiciona en el usuario (~/) y las webs están en: ~/public_html/carpetaDominio/

Trabajamos en local

Hacemos un push
```
$ git push origin master
```

Desde server desplegamos en una carpeta el proyecto git
```
$ git clone ruta_del_repo.git ruta_de_destino
```

La ruta_de_destino es la carpeta donde se guardarán los ficheros de la web

A partir de aquí, luego de algún push de local, hacemos pull desde el server:
```
$ git pull origin master
```


## GIT TAGS

[Git Tags](http://miguelnieva.github.io/presentacion-workshop-3/#/)

Especificar momentos de tu repositorio importantes, generalmente versiones del proyecto, un feature complejo, etc.

Un TAG es un commit "abanderado"

Existen 2 formas de crear tags. 

- Annotated Tags 
- Lightweight Tags


**Annotated Tag**: Agregas la versión y un mensaje para agregar, al ejecutarlo se agrega el TAG al COMMIT donde está ubicado el HEAD.
```
$ git tag -a [versión] -m "[mensaje]"
$ git tag -a v1.0 -m "Navegación completada"
```

Muestra listado de tags creados.
```
$ git tag
```

Busca la versión que le solicites.
```
$ git tag -l "[versión]"
```

Mover el HEAD a un commit por nombre de tag
```
$ git checkout [versión]
```

Poner tag a un commit ya creado:
```
// Opción 1: navegar hasta el commit y ponerle tag normal:
(168dc2d)$ git tag -a [versión] -m "[mensaje]"

// Opción 2: comando normal pero al final ponemos el ID del commit: 
(master)$ git tag -a [versión] -m "[mensaje]" 168dc2d
```

**Lightweight Tags**: solo se indica el nombre del tag sin -a
```
$ git tag [versión]
$ git tag v1.0
```


## GIT EXTRAS

Es una Extensión de GIT para tener más comandos.

[Git Extras](https://github.com/tj/git-extras)

* Tener [Git for Windows](https://github.com/git-for-windows/git/releases) instalado.

* Clonar repo de [git-extras](https://github.com/tj/git-extras) en cualquier directorio.
```
$ git clone https://github.com/tj/git-extras.git
# checkout the latest tag (optional)
$ git checkout $(git describe --tags $(git rev-list --tags --max-count=1))
```
Por último, ejecutar **install.cmd** desde la consola cmd.

NOTA:  Todavía no lo he probado


## GIT GUI

Interfaces gráficas de GIT en Windows:

[SourceTree](https://www.sourcetreeapp.com/)

[Github Desktop](https://desktop.github.com/)


## ZENHUB

Aplicación para Proyect Management más completo y de manera nativa desde Github.

[ZenHub](https://www.zenhub.com/)

Es una extensión al navegador y que da funcionalidades adicionales dentro de Github web.

**Pestaña Boards**

Son columnas (pipes) con nombres que simulan el estado del proyecto: 
New Issues(nuevos), Icebox(ideas), Backlog(prioritarios), In Progress(trabajando), Review(revisión), Done(finalizado), ya puedes agregar más pipes.

**Epics**

Agrega un nivel más de agrupamiento de Issues. Puedes agrupar Issues relacionados dentro de Milestone, por lo que te puede quedar: Proyecto >> Milestone >> Epics >> Issues

**Pestaña Burndown**

Muestra una gráfica del tiempo total de gestión del proyecto.


## GITLAB

Instala toda una comunidad como Github en tu servidor, con interface gráfica.

[GitLab](https://about.gitlab.com/)

Los términos cambian un poco:

| GIT HUB       | GIT LAB       | Que significa            |
|------------ --|---------------|--------------------------|
| Pull Request  | Merge Request | Petición de Fusión       |
| Gist          | Snippet       | "Pedazos" de código      |
| Repository    | Project       | "Repo" de Git            |
| Organizations | Groups        | "Group-Level" Management |


##### FIN