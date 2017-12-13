

# Config
```shell
git config --global user.name "Adria"
git config --global user.email "adriamoya@gmail.com"

git config --global -e
```
# Initialize

Crea una carpeta .git (repositorio)
```shell
git init
git status
```
Añadir todos los archivos para subir (ver de nuevo `git status`)
```shell
git add .
```
Subir snapshot con mensaje
```shell
git commit -m "mensaje"
```

Recuperar cambios del último commit
```shell
git checkout -- .
```
Git sabe que se han realizado cambios y es capaz de volver atrás.

Log de Git
```shell
git log
```

# Añadir archivos (enviar archivos al stage)

Un único archivo
```shell
git add index.html
```
Todos los archivos con una misma extensión
```shell
git add *.png
git status
```
Todos los archivos dentro de una carpeta
```shell
git add css/
git status
```
Todos los archivos que fueron modificados
```shell
git add -A
```
Excluir archivos del stage
```shell
git reset *.xml
git status
```
Todos los archivos .txt que hayamos modificado en TODO el proyecto
```shell
git add "*.txt"
```
Todos los archivos .txt en el directorio actual
```shell
git add *.txt
```
Agrega todos los archivos que se hayan modificado
```shell
git add .
git add --all
git add -A
```
Todos los archivos que listemos
```shell
git add <list de archivos>
```
Agrega todos los PDFs dentro de la carpeta documentos
```shell
git add documentos/*.pdf
```
Agrega todos los archivos de la carpeta documentos
```shell
git add documentos/
```

# Log

El HEAD es el último `commit` en la rama actual o la línea del tiempo en la cual estamos trabajando.

De forma más compacta
```shell
git log --oneline
```
```shell
git log --oneline --decorate --all --graph
```

# Status

Silent mode
```shell
git status -s
```

Para ver en la rama en la que estamos trabajando
```shell
git status -s -b
```

# Alias

Configuraremos los alias de forma global
```shell
git config --global alias.lg "log --oneline --decorate --all --graph"
git lg

git config --global alias.s "status -s -b"
git s
```

```shell
git config --global -l
```

# Diferencias entre commits y restauración de archivos

Cambios que han sufrido los archivos desde el último `commit`
```shell
git diff
```

Cambios que han sufrido los archivos desde el último `commit`y que ya están en el stage
```shell
git diff --staged
```

Para quitar el archivo del stage
```shell
git reset HEAD <nombre del archivo>
git status
```

Para revertir cambios de un archivo específico
```shell
git checkout -- <nombre del archivo>
```

# Actualizar mensaje commit y revertir commits

Agregar todos los archivos al stage y hacer commit:
```shell
git commit -am "<mensaje del commit>""
```

Cambiar mensaje del commit
```shell
git commit --amend -m "<nuevo mensaje>"
```

También como 
```shell
git commit --ammend
```
Podemos escribir el mensaje en insert mode `i` y para salir y guardar cambios `Esc` y escribimos `:wq`.

Añadir más archivos al commit ya creado (HEAD apunta al commit anterior)
```shell
git reset --soft HEAD^
git status
git commit -am "<mensaje>"
```

Para cambiar cualquier otro commit (movernos a cualquier punto de la historia):
```shell
git lg  # copiamos el nombre del HEAD
git reset -soft <nombre del HEAD>
```
Realizamos las modificaciones necesarias
```shell
git status
```

Podemos quitar los archivos del stage y perder todos los cambios utilizando `--hard` en el `git reset` para un HEAD específico:
```shell
git reset --hard <nombre del HEAD>
git lg
git s
```

Podemos regresar si hemos hechos resets hards (volver al futuro)
```shell
git reflog  # copiar el HEAD deseado
```
```shell
git reset --hard <nombre del HEAD>
git lg
```

# Cambiar el nombre y eliminar archivos

```shell
git mv <nombre actual> <nombre nuevo>
git s
git commit -m "<mensaje>"
```

Para eliminar un archivo
```shell
git rm <nombre del archivo>
git s
git commit -m "<mensaje>"
```

# Gitignore

Creamos archivo `.gitignore`:

```shell
data.log 		# no incluir data.log
*.log 			# no incluir ningún archivo .log
node_modules/	# no incluir directorio
```

```shell
git s
git add -A
git commit -m "Agregar gitignore"
```

# Ramas

Líneas paralelas en el tiempo.

Merge:

  * fast-forward (no ha habido commits en el master y se puede hacer un merge limpio de la rama al aster)
  * uniones automáticas (git reconoce que en el master ha habido algún commit que la rama desconoce)
  * uniones manuales (en la rama hicimos modificaciones que hacen conflicto con mismas modificaciones que hicimos en la rama master --> merge commit)

### Fast-forward

```shell
git branch <nombre de la rama>
```
Para ver las ramas (en verde donde nos encontramos):
```shell
git branch
```
Para movernos a una rama
```shell
git checkout <nombre de la rama>
git s
```

Ahora podemos hacer commits en la rama

Si queremos volver e integrar los commits de la rama al master

```shell
git diff <rama> <master>  # para ver los cambios entre la rama y el master
```

Regresamos a la rama donde queremos fusionar algo (como queremos añadir una rama al master, vamos al master)
```shell
git checkout master
```

```shell
git merge <nombre de la rama>
```

Para eliminar la rama
```shell
git branch
git branch -d <nombre de la rama>
git branch
```

# Remote

```shell
git remote add origin <http/ssh del github repository>
```

```shell
git remote
git remote -v  # indica cual es el fetch y el push
```

# Push

```shell
git push -u origin master
```

Un `git push`no sube por defecto los tags
```shell
git push --tags
```

# Pull

Actualiza los cambios a local:

```shell
git pull  # como ya hicimos -u en el push, no hace falta hacer git pull origin master
```

# Clone

Clonar todo un repositorio

```shell
git clone <http/ssh del github repository> <nuevo nombre del repositorio en local>
```

# Fetch vs Pull

Si hacemos cambios en paralelo, cuando se intente hacer un commit desde cualquier terminal aparecerá el mensaje AHEAD (ver `git status`). Por ejemplo: 
`Your branch is ahead of 'origin/master' by 1 commit (use git push to publish your local commits). Nothing to commit, working tree clean`.

Si hacemos el `push` podemos obtener errores que sugieran que el `remote` tiene cambios que no están en local y que hagamos un `git pull` antes de pushing.

Si hacemos un `git pull`, git intentará hacer un merge de forma automática y podriamos perder los cambios en local. Por ello es mejor realizar un `git fetch` para actualizar en el repositorio local todos los cambios que hayan sucedido en el repositorio remoto.

Si hacemos de nuevo un `git status`, seguramente seguirá diciendo que hay una diferencia por un commit y nos recomendará que hagamos un `git merge` o un `git pull`. Normalmente, `git pull` será más rápido. Si hay algún conflicto deberá ser resuelto de forma manual y, si todo va bien, aparecerá un prompt de vi para poner nombre al merge.

Ahora se puede hacer un `git push` para que github tenga los cambios que hicimos en local.


While the `git fetch` command will fetch down all the changes on the server that you don’t have yet, it will not modify your working directory at all. It will simply get the data for you and let you merge it yourself. However, there is a command called `git pull` which is essentially a `git fetch` immediately followed by a `git merge` in most cases. If you have a tracking branch set up, either by explicitly setting it or by having it created for you by the clone or checkout commands, `git pull` will look up what server and branch your current branch is tracking, fetch from that server and then try to merge in that remote branch.

Generally it’s better to simply use the `fetch` and `merge` commands explicitly as the magic of `git pull` can often be confusing.






































