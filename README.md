# GRUPO4DAW26 · Comandos usados

Resumen de los comandos de Git y de terminal que se han utilizado para gestionar el repositorio, fusionar las ramas y dejar todo en `main`.

- **Repositorio:** https://github.com/victorsc-06/GRUPO4DAW26
- **Ramas de trabajo:** `view` (vista y front-end), `Model` (modelo) y `controller` (controlador, configuración e index).
- **Herramientas:** GitHub Desktop + Command Prompt (se abre con *Repository → Open in Command Prompt*).

---

## 1. Comprobar el estado del repositorio

```bash
git status
git remote -v
git log --oneline --graph -10
git ls-files
git log origin/view --oneline -5
git log origin/Model --oneline -5
git log origin/controller --oneline -5
```

- `git status`: muestra si hay cambios pendientes y si la rama está al día.
- `git remote -v`: muestra la URL del repositorio remoto.
- `git log ...`: muestra los últimos commits.
- `git ls-files`: lista los archivos que contiene la rama actual.

## 2. Renombrar el repositorio (GRUPO4DAW25 → GRUPO4DAW26)

Primero en GitHub: *Settings → General → Repository name → Rename*. Después, en local:

```bash
git remote set-url origin https://github.com/victorsc-06/GRUPO4DAW26.git
```

## 3. Descargar novedades de GitHub

```bash
git fetch origin
git pull origin main
```

- `git fetch origin`: descarga los cambios de GitHub sin aplicarlos.
- `git pull origin main`: descarga y aplica los cambios de `main`.

## 4. Fusionar las ramas en `main`

Las ramas se crearon por separado y no compartían historia, por lo que GitHub daba el error **"unrelated histories"**. Se resolvió con `--allow-unrelated-histories`.

```bash
git checkout main
git fetch origin
git merge origin/view --allow-unrelated-histories -m "Fusionar view"
git merge origin/Model --allow-unrelated-histories -m "Fusionar Model"
git merge origin/controller --allow-unrelated-histories -m "Fusionar controller"
git push origin main
```

Una vez que las ramas ya comparten historia, ya no hace falta la opción:

```bash
git fetch origin
git merge origin/controller -m "Fusionar controller actualizado"
git push origin main
```

## 5. Resolver conflictos

**Conflicto de `README.md`** (borrado en `controller` y modificado en `main`): se conservó la versión de `main`.

```bash
git add README.md
git commit -m "Fusionar controller"
git push origin main
```

**Conflicto en un archivo en el que se quiere la versión de la otra rama** (ejemplo: `app/Config.php`):

```bash
git checkout --theirs app/Config.php
git add app/Config.php
git commit -m "Fusionar controller actualizado"
git push origin main
```

## 6. Subir archivos propios (valoraciones.txt)

```bash
notepad valoraciones.txt
git add valoraciones.txt
git commit -m "Añadir plantilla de valoraciones"
git push origin main
```

Para completarlo cuando los compañeros escriban su parte:

```bash
git checkout main
git pull origin main
```

Si hay cambios locales que chocan y se quiere quedarse con la versión de GitHub:

```bash
git checkout -- valoraciones.txt
git pull origin main
```

## 7. Error `ORIG_HEAD` / `index.lock` (OneDrive)

Al hacer `git pull` apareció: `cannot lock ref 'ORIG_HEAD': reference broken`. Se arregló borrando los archivos internos bloqueados:

```bash
del .git\ORIG_HEAD
del .git\index.lock
git pull origin main
```

**Recomendación:** el repositorio está dentro de OneDrive, que sincroniza `.git` mientras Git trabaja. Conviene pausar OneDrive o clonar el repo en una carpeta fuera de él (por ejemplo `C:\GitHub\`).

## 8. Comprobación final

```bash
git status
```

Debe indicar `Your branch is up to date with 'origin/main'` y `nothing to commit, working tree clean`. Después, en GitHub (rama `main`), comprobar que existen:

- `app/`: `Model.php`, `Controller.php`, `Config.php`, `templates/`
- `web/`: `index.php`, `css/`, `js/`, `images/`
- `valoraciones.txt`, `instalación.txt` e `histórico.txt`

---

## Resumen rápido

| Comando | Para qué sirve |
|---|---|
| `git status` | Ver el estado de la rama |
| `git remote -v` | Ver la URL del remoto |
| `git remote set-url origin URL` | Cambiar la URL del remoto |
| `git fetch origin` | Descargar novedades sin aplicarlas |
| `git pull origin main` | Descargar y aplicar cambios de `main` |
| `git checkout main` | Cambiar a la rama `main` |
| `git merge origin/RAMA -m "mensaje"` | Fusionar una rama en la actual |
| `--allow-unrelated-histories` | Permitir fusionar ramas sin historia común |
| `git add ARCHIVO` | Preparar un archivo para el commit |
| `git commit -m "mensaje"` | Guardar los cambios preparados |
| `git push origin main` | Subir `main` a GitHub |
| `git checkout --theirs ARCHIVO` | En un conflicto, quedarse con la versión de la otra rama |
| `del .git\ORIG_HEAD` | Borrar un archivo interno de Git corrupto |
