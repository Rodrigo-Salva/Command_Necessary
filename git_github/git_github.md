# Guía Completa de Git y GitHub - Comandos Esenciales

## 📋 Tabla de Contenidos
- [Configuración Inicial](#configuración-inicial)
- [Comandos Básicos](#comandos-básicos)
- [Trabajo con Ramas (Branches)](#trabajo-con-ramas-branches)
- [Trabajo con Repositorios Remotos](#trabajo-con-repositorios-remotos)
- [Historial y Logs](#historial-y-logs)
- [Deshacer Cambios](#deshacer-cambios)
- [Stashing (Guardar Cambios Temporalmente)](#stashing-guardar-cambios-temporalmente)
- [Tagging (Etiquetas)](#tagging-etiquetas)
- [Merge y Rebase](#merge-y-rebase)
- [Comandos de Inspección](#comandos-de-inspección)
- [GitHub CLI](#github-cli)
- [Comandos Avanzados](#comandos-avanzados)
- [Trucos y Aliases Útiles](#trucos-y-aliases-útiles)

---

## 🔧 Configuración Inicial

### Configurar identidad
```bash
# Configurar nombre de usuario globalmente
git config --global user.name "Tu Nombre"

# Configurar email globalmente
git config --global user.email "tu@email.com"

# Ver toda la configuración
git config --list

# Ver configuración específica
git config user.name
git config user.email
```

### Configurar editor por defecto
```bash
# Configurar VS Code como editor
git config --global core.editor "code --wait"

# Configurar Vim como editor
git config --global core.editor vim

# Configurar Nano como editor
git config --global core.editor nano
```

### Configuraciones útiles
```bash
# Configurar colores en terminal
git config --global color.ui auto

# Configurar line endings (para Windows)
git config --global core.autocrlf true

# Configurar line endings (para macOS/Linux)
git config --global core.autocrlf input

# Configurar push por defecto
git config --global push.default simple

# Configurar pull por defecto
git config --global pull.rebase false
```

---

## 🚀 Comandos Básicos

### Inicializar y clonar repositorios
```bash
# Inicializar un repositorio nuevo
git init

# Clonar un repositorio existente
git clone https://github.com/usuario/repositorio.git

# Clonar a una carpeta específica
git clone https://github.com/usuario/repositorio.git mi-proyecto

# Clonar solo la rama principal (más rápido)
git clone --single-branch https://github.com/usuario/repositorio.git
```

### Estados y área de staging
```bash
# Ver estado actual del repositorio
git status

# Ver estado de forma compacta
git status -s

# Agregar archivo específico al staging
git add archivo.txt

# Agregar todos los archivos modificados
git add .

# Agregar todos los archivos (incluye eliminados)
git add -A

# Agregar archivos de forma interactiva
git add -i

# Agregar partes específicas de un archivo
git add -p archivo.txt
```

### Commits
```bash
# Hacer commit con mensaje
git commit -m "Mensaje del commit"

# Commit agregando archivos modificados automáticamente
git commit -am "Mensaje del commit"

# Commit con editor para mensaje largo
git commit

# Modificar el último commit (agregar cambios o cambiar mensaje)
git commit --amend

# Modificar último commit sin cambiar mensaje
git commit --amend --no-edit

# Commit vacío (útil para triggers)
git commit --allow-empty -m "Trigger deployment"
```

### Diferencias
```bash
# Ver diferencias no agregadas al staging
git diff

# Ver diferencias en staging
git diff --cached

# Ver diferencias de un archivo específico
git diff archivo.txt

# Ver diferencias entre commits
git diff commit1 commit2

# Ver diferencias entre ramas
git diff rama1 rama2

# Ver estadísticas de diferencias
git diff --stat
```

---

## 🌿 Trabajo con Ramas (Branches)

### Crear y cambiar ramas
```bash
# Ver todas las ramas locales
git branch

# Ver todas las ramas (locales y remotas)
git branch -a

# Crear nueva rama
git branch nueva-rama

# Crear y cambiar a nueva rama
git checkout -b nueva-rama

# Cambiar a rama existente
git checkout nombre-rama

# Crear rama desde commit específico
git checkout -b nueva-rama abc1234

# Cambiar a rama (comando moderno)
git switch nombre-rama

# Crear y cambiar a nueva rama (comando moderno)
git switch -c nueva-rama
```

### Gestionar ramas
```bash
# Renombrar rama actual
git branch -m nuevo-nombre

# Renombrar rama específica
git branch -m viejo-nombre nuevo-nombre

# Eliminar rama local
git branch -d nombre-rama

# Forzar eliminación de rama local
git branch -D nombre-rama

# Eliminar rama remota
git push origin --delete nombre-rama

# Ver ramas fusionadas
git branch --merged

# Ver ramas no fusionadas
git branch --no-merged
```

---

## 🌐 Trabajo con Repositorios Remotos

### Configurar remotos
```bash
# Ver repositorios remotos
git remote

# Ver repositorios remotos con URLs
git remote -v

# Agregar repositorio remoto
git remote add origin https://github.com/usuario/repo.git

# Cambiar URL de remoto existente
git remote set-url origin https://github.com/usuario/nuevo-repo.git

# Eliminar repositorio remoto
git remote remove origin

# Renombrar repositorio remoto
git remote rename origin upstream
```

### Push y Pull
```bash
# Subir cambios al remoto
git push origin main

# Subir primera vez (configurar upstream)
git push -u origin main

# Subir todas las ramas
git push --all

# Subir tags
git push --tags

# Forzar push (¡cuidado!)
git push --force

# Forzar push más seguro
git push --force-with-lease

# Descargar cambios sin fusionar
git fetch

# Descargar de remoto específico
git fetch origin

# Descargar y fusionar cambios
git pull

# Pull con rebase
git pull --rebase

# Pull de rama específica
git pull origin rama-especifica
```

---

## 📜 Historial y Logs

### Ver historial
```bash
# Ver historial completo
git log

# Ver historial compacto (una línea por commit)
git log --oneline

# Ver historial con gráfico
git log --graph

# Ver historial bonito
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

# Ver últimos n commits
git log -5

# Ver historial de archivo específico
git log -- archivo.txt

# Ver historial con diferencias
git log -p

# Ver estadísticas de cambios
git log --stat

# Ver commits de autor específico
git log --author="Nombre"

# Ver commits en rango de fechas
git log --since="2023-01-01" --until="2023-12-31"

# Ver commits que contienen texto específico
git log --grep="fix"
```

### Otros comandos de historial
```bash
# Ver información de commit específico
git show abc1234

# Ver archivos en commit específico
git show --name-only abc1234

# Ver resumen de cambios
git shortlog

# Ver historial reflog (muy útil para recuperar trabajo)
git reflog

# Buscar texto en todo el historial
git log -S "texto_a_buscar"

# Ver quién modificó cada línea de un archivo
git blame archivo.txt
```

---

## ↩️ Deshacer Cambios

### Deshacer cambios no committeados
```bash
# Descartar cambios en archivo específico
git checkout -- archivo.txt

# Descartar cambios en archivo específico (comando moderno)
git restore archivo.txt

# Descartar todos los cambios no committeados
git checkout -- .

# Descartar todos los cambios (comando moderno)
git restore .

# Sacar archivo del staging
git reset HEAD archivo.txt

# Sacar archivo del staging (comando moderno)
git restore --staged archivo.txt

# Sacar todos los archivos del staging
git reset HEAD

# Limpiar archivos no tracked
git clean -f

# Limpiar archivos y directorios no tracked
git clean -fd

# Ver qué se limpiará sin hacerlo
git clean -n
```

### Deshacer commits
```bash
# Deshacer último commit manteniendo cambios
git reset --soft HEAD~1

# Deshacer último commit y sacar cambios del staging
git reset HEAD~1

# Deshacer último commit y descartar cambios (¡cuidado!)
git reset --hard HEAD~1

# Deshacer múltiples commits
git reset --soft HEAD~3

# Revertir commit específico (crea nuevo commit)
git revert abc1234

# Revertir merge commit
git revert -m 1 abc1234

# Volver a commit específico manteniendo historial
git revert HEAD~2..HEAD
```

---

## 📦 Stashing (Guardar Cambios Temporalmente)

### Comandos básicos de stash
```bash
# Guardar cambios actuales en stash
git stash

# Guardar con mensaje descriptivo
git stash save "Trabajo en progreso en feature X"

# Guardar incluyendo archivos no tracked
git stash -u

# Ver lista de stashes
git stash list

# Aplicar último stash
git stash apply

# Aplicar stash específico
git stash apply stash@{0}

# Aplicar y eliminar último stash
git stash pop

# Aplicar y eliminar stash específico
git stash pop stash@{1}

# Eliminar stash específico
git stash drop stash@{0}

# Eliminar todos los stashes
git stash clear

# Ver contenido de stash
git stash show

# Ver diferencias detalladas de stash
git stash show -p

# Crear rama desde stash
git stash branch nueva-rama stash@{0}
```

---

## 🏷️ Tagging (Etiquetas)

### Crear y gestionar tags
```bash
# Crear tag ligero
git tag v1.0.0

# Crear tag anotado
git tag -a v1.0.0 -m "Versión 1.0.0"

# Crear tag en commit específico
git tag -a v1.0.0 abc1234

# Ver todas las tags
git tag

# Ver tags con patrón
git tag -l "v1.*"

# Ver información de tag específico
git show v1.0.0

# Subir tag específico
git push origin v1.0.0

# Subir todas las tags
git push origin --tags

# Eliminar tag local
git tag -d v1.0.0

# Eliminar tag remoto
git push origin :refs/tags/v1.0.0

# O eliminar tag remoto (sintaxis moderna)
git push origin --delete v1.0.0
```

---

## 🔀 Merge y Rebase

### Merge
```bash
# Fusionar rama en rama actual
git merge nombre-rama

# Merge sin fast-forward (siempre crea commit de merge)
git merge --no-ff nombre-rama

# Merge con estrategia específica
git merge -X theirs nombre-rama

# Cancelar merge en progreso
git merge --abort

# Continuar merge después de resolver conflictos
git merge --continue
```

### Rebase
```bash
# Rebase rama actual sobre otra rama
git rebase main

# Rebase interactivo (para modificar historial)
git rebase -i HEAD~3

# Rebase interactivo desde commit específico
git rebase -i abc1234

# Continuar rebase después de resolver conflictos
git rebase --continue

# Saltar commit problemático durante rebase
git rebase --skip

# Cancelar rebase
git rebase --abort

# Rebase preservando merges
git rebase -p main
```

### Cherry-pick
```bash
# Aplicar commit específico a rama actual
git cherry-pick abc1234

# Cherry-pick múltiples commits
git cherry-pick abc1234 def5678

# Cherry-pick rango de commits
git cherry-pick abc1234..def5678

# Cherry-pick sin commit automático
git cherry-pick -n abc1234

# Cancelar cherry-pick
git cherry-pick --abort

# Continuar cherry-pick
git cherry-pick --continue
```

---

## 🔍 Comandos de Inspección

### Buscar y encontrar
```bash
# Buscar texto en archivos tracked
git grep "texto_a_buscar"

# Buscar en commit específico
git grep "texto" abc1234

# Buscar con número de línea
git grep -n "texto"

# Buscar ignorando mayúsculas
git grep -i "texto"

# Buscar archivos que contengan texto
git grep -l "texto"

# Encontrar commit que introdujo un bug
git bisect start
git bisect bad          # commit actual tiene el bug
git bisect good abc1234 # este commit estaba bien

# Continuar bisect
git bisect good  # o git bisect bad

# Terminar bisect
git bisect reset
```

### Información del repositorio
```bash
# Ver información del repositorio
git remote show origin

# Ver estadísticas del repositorio
git count-objects -v

# Ver tamaño del repositorio
du -sh .git

# Ver archivos más grandes en historial
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | awk '/^blob/ {print substr($0,6)}' | sort -k3 -n | tail -10

# Ver contribuciones por autor
git shortlog -s -n

# Ver actividad por fechas
git log --format='%ad' --date=short | sort | uniq -c
```

---

## 🐙 GitHub CLI

### Instalación y autenticación
```bash
# Autenticarse en GitHub
gh auth login

# Ver estado de autenticación
gh auth status

# Configurar git para usar GitHub CLI
gh auth setup-git
```

### Repositorios
```bash
# Clonar repositorio propio
gh repo clone mi-repo

# Clonar repositorio de otro usuario
gh repo clone usuario/repo

# Crear repositorio
gh repo create mi-nuevo-repo

# Crear repositorio público
gh repo create mi-repo --public

# Crear repositorio privado
gh repo create mi-repo --private

# Ver información del repositorio
gh repo view

# Fork de repositorio
gh repo fork usuario/repo

# Listar repositorios propios
gh repo list

# Listar repositorios de usuario
gh repo list usuario
```

### Issues
```bash
# Listar issues
gh issue list

# Crear nuevo issue
gh issue create

# Ver issue específico
gh issue view 123

# Cerrar issue
gh issue close 123

# Reabrir issue
gh issue reopen 123

# Asignar issue
gh issue edit 123 --assignee @me
```

### Pull Requests
```bash
# Crear pull request
gh pr create

# Listar pull requests
gh pr list

# Ver pull request específico
gh pr view 123

# Hacer checkout de PR
gh pr checkout 123

# Fusionar pull request
gh pr merge 123

# Cerrar pull request
gh pr close 123

# Revisar pull request
gh pr review 123

# Ver diferencias de PR
gh pr diff 123
```

### Releases
```bash
# Listar releases
gh release list

# Crear release
gh release create v1.0.0

# Ver release específico
gh release view v1.0.0

# Descargar assets de release
gh release download v1.0.0

# Eliminar release
gh release delete v1.0.0
```

---

## ⚡ Comandos Avanzados

### Submodules
```bash
# Agregar submodule
git submodule add https://github.com/usuario/repo.git ruta/submodule

# Inicializar submodules después de clonar
git submodule init
git submodule update

# O en un solo comando
git submodule update --init --recursive

# Actualizar submodules
git submodule update --remote

# Ver estado de submodules
git submodule status

# Eliminar submodule
git submodule deinit ruta/submodule
git rm ruta/submodule
```

### Worktree
```bash
# Crear worktree en nueva ubicación
git worktree add ../feature-branch feature-branch

# Listar worktrees
git worktree list

# Eliminar worktree
git worktree remove ../feature-branch

# Limpiar worktrees eliminados
git worktree prune
```

### Hooks
```bash
# Ver hooks disponibles
ls .git/hooks/

# Hacer hook ejecutable
chmod +x .git/hooks/pre-commit

# Hook global
git config --global core.hooksPath ~/.git-hooks
```

### Configuración avanzada
```bash
# Configurar GPG signing
git config --global user.signingkey YOUR_KEY_ID
git config --global commit.gpgsign true

# Configurar alias
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit

# Configurar rerere (reutilizar resoluciones de conflictos)
git config --global rerere.enabled true

# Configurar autosquash
git config --global rebase.autosquash true
```

---

## 🎯 Trucos y Aliases Útiles

### Aliases útiles para .gitconfig
```bash
[alias]
    # Status y log
    st = status -s
    lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
    last = log -1 HEAD
    
    # Branches
    co = checkout
    br = branch
    sw = switch
    
    # Commits
    ci = commit
    amend = commit --amend
    undo = reset HEAD~1 --mixed
    
    # Remote
    up = !git remote update -p; git merge --ff-only @{u}
    pushf = push --force-with-lease
    
    # Diff
    df = diff
    dc = diff --cached
    
    # Stash
    sl = stash list
    sa = stash apply
    ss = stash save
    
    # Utils
    aliases = config --get-regexp alias
    contributors = shortlog --summary --numbered
    ignore = "!gi() { curl -L -s https://www.gitignore.io/api/$@ ;}; gi"
```

### Comandos útiles combinados
```bash
# Ver archivos modificados en último commit
git diff-tree --no-commit-id --name-only -r HEAD

# Crear archivo .gitignore desde gitignore.io
curl -L -s https://www.gitignore.io/api/python,node,visualstudiocode > .gitignore

# Limpiar ramas locales que ya fueron fusionadas
git branch --merged main | grep -v "\* main" | xargs -n 1 git branch -d

# Ver commits no pusheados
git log @{u}..HEAD

# Ver commits en remoto que no tenemos
git log HEAD..@{u}

# Buscar texto en todo el historial
git log -S "search term" --source --all

# Ver tamaño de archivos en repositorio
git ls-tree -r -t -l --full-name HEAD | sort -n -k 4

# Exportar branch como archivo
git archive --format zip --output /path/to/file.zip main
```

### Variables de entorno útiles
```bash
# Configurar editor temporal
export GIT_EDITOR=vim

# Configurar merge tool temporal
export GIT_MERGE_TOOL=vimdiff

# Deshabilitar pager para comandos específicos
git --no-pager log --oneline -10
```

---

## 🚨 Comandos de Emergencia

### Recuperar trabajo perdido
```bash
# Ver todo lo que ha pasado en el repositorio
git reflog

# Recuperar rama eliminada
git checkout -b rama-recuperada abc1234

# Recuperar archivo eliminado
git checkout abc1234 -- archivo.txt

# Buscar commit perdido
git fsck --unreachable

# Recuperar commit específico
git cherry-pick abc1234
```

### Limpiar repositorio
```bash
# Limpiar archivos no tracked
git clean -fd

# Resetear todo al último commit
git reset --hard HEAD

# Limpiar todo incluyendo archivos ignored
git clean -fdx

# Garbage collection manual
git gc --aggressive --prune=now
```

### Resolver problemas comunes
```bash
# Cambiar mensaje de commit ya pusheado (¡cuidado!)
git commit --amend -m "Nuevo mensaje"
git push --force-with-lease

# Deshacer push accidental
git reset --hard HEAD~1
git push --force-with-lease

# Resolver "detached HEAD"
git checkout main
git branch temp-branch abc1234  # si quieres guardar cambios

# Resolver conflictos de merge
git status  # ver archivos en conflicto
# editar archivos
git add archivo-resuelto.txt
git commit
```

---

## 📱 Flujos de Trabajo Comunes

### Git Flow básico
```bash
# Crear feature branch
git checkout -b feature/nueva-funcionalidad

# Trabajar y commitear
git add .
git commit -m "Implementar nueva funcionalidad"

# Pushear feature branch
git push -u origin feature/nueva-funcionalidad

# Mergear a main via PR/MR o localmente:
git checkout main
git merge feature/nueva-funcionalidad
git push origin main

# Limpiar
git branch -d feature/nueva-funcionalidad
git push origin --delete feature/nueva-funcionalidad
```

### Hotfix rápido
```bash
# Crear hotfix desde main
git checkout main
git pull origin main
git checkout -b hotfix/bug-critico

# Fix y commit
git add .
git commit -m "Fix: corregir bug crítico"

# Push y PR/merge rápido
git push -u origin hotfix/bug-critico
```

Esta guía cubre todos los comandos esenciales de Git y GitHub que necesitas para el trabajo diario como programador. Guárdala como referencia y practica los comandos regularmente para dominar el flujo de trabajo con Git.