# 📘 Guía de Referencia de Git y GitHub

Manual técnico de comandos esenciales ordenados por flujo de trabajo para la administración de repositorios locales y remotos.

---

## 1. Configuración de Identidad
Establece las credenciales del usuario en el entorno local.
* `git config --global user.name "Tu Nombre"` -> Define el nombre del usuario global.
* `git config --global user.email "tu@email.com"` -> Define el correo electrónico global.
* `git config --list` -> Lista todas las variables de configuración activas.

---

## 2. Inicialización y Vinculación Remota
Comandos para crear un repositorio local y conectarlo con un servidor remoto como GitHub.
* `git init` -> Inicializa un nuevo repositorio Git en el directorio actual.
* `git remote add origin <URL_DEL_REPOSITORIO>` -> Vincula el repositorio local a una URL remota bajo el alias `origin`.
* `git remote -v` -> Muestra las URLs de los remotos configurados para lectura (fetch) y escritura (push).
* `git remote remove <nombre_remoto>` -> Elimina el enlace remoto especificado.
* `git remote set-url origin <NUEVA_URL>` -> Modifica la URL de un control remoto existente.

---

## 3. Flujo de Trabajo Básico y Confirmaciones
Ciclo estándar para registrar modificaciones en el historial de cambios.
* `git status` -> Muestra el estado de los archivos en el directorio de trabajo (sin rastrear, modificados o en staging).
* `git add .` -> Sigue y añade todos los archivos modificados al área de preparación (Staging).
* `git add <archivo>` -> Añade un archivo específico al área de preparación.
* `git commit -m "Mensaje descriptivo"` -> Consolida los cambios del área de preparación en el historial local.

---

## 4. Gestión y Navegación de Ramas
Comandos para administrar las diferentes líneas de desarrollo de un proyecto.
* `git branch` -> Lista las ramas locales. La rama activa se resalta con un asterisco (`*`).
* `git branch -M main` -> Renombra de forma forzada la rama actual (comúnmente usado para migrar de `master` a `main`).
* `git switch <nombre_rama>` -> Cambia el entorno de trabajo a una rama local existente.
* `git checkout <nombre_rama>` -> Comando alternativo para cambiar a una rama existente.
* `git switch -c <nueva_rama>` -> Crea una rama nueva y se desplaza a ella en un solo paso.
* `git checkout -b <nueva_rama>` -> Comando alternativo para crear y cambiar a una rama nueva.
* `git branch -d <nombre_rama>` -> Elimina una rama local (requiere que sus cambios ya estén integrados).

---

## 5. Fusión de Ramas e Integración de Código (`git merge`)
Combina el historial de desarrollo de una rama secundaria dentro de una rama objetivo. Identifica el ancestro común de ambas ramas, junta los cambios de forma automática si ocurren en líneas distintas y genera un nuevo commit de fusión que unifica ambas líneas de desarrollo.

> 🌐 **Nota operativa:** Los comandos de fusión deben ejecutarse siempre posicionándose primero en la rama receptora de los cambios.

1. `git switch main` -> Cambia a la rama principal receptora.
2. `git pull origin main` -> Descarga e integra los últimos cambios remotos para evitar desfases.
3. `git merge <rama_secundaria>` -> Fusiona el historial de la rama secundaria en la rama activa.

### Resolución de Conflictos de Fusión
Si el proceso de fusión se detiene por modificaciones concurrentes en las mismas líneas de un archivo:
1. Inspeccione los archivos afectados en el editor de texto.
2. Elimine las marcas delimitadoras (`<<<<<<<`, `=======`, `>>>>>>>`) seleccionando el código definitivo.
3. Guarde los archivos editados.
4. Concluya la fusión registrando los cambios:
   ```bash
   git add .
   git commit -m "Resolución de conflictos de fusión"
   ```
* `git merge --abort` -> Cancela por completo el proceso de fusión y devuelve el proyecto a su estado original anterior al conflicto.

---

## 6. Sincronización con Repositorios Remotos
Envío y descarga de datos entre el entorno local y el servidor.
* `git push -u origin <nombre_rama>` -> Sube la rama local al servidor remoto por primera vez y establece la relación de seguimiento (upstream).
* `git push` -> Sube los commits locales de la rama activa (válido tras configurar el upstream inicial).
* `git pull origin <nombre_rama>` -> Descarga e incorpora los cambios del repositorio remoto directamente en la rama actual (ejecuta un `fetch` seguido de un `merge` automático).
* `git fetch origin` -> Descarga la información, commits y ramas de GitHub al entorno local **sin modificar tus archivos actuales ni tu código**. Permite inspeccionar cambios remotos antes de integrarlos.
* `git diff main origin/main` -> Compara las diferencias entre la rama local `main` y la rama remota descargada mediante `fetch`.
* `git clone <URL_DEL_REPOSITORIO>` -> Descarga una copia completa de un repositorio remoto en un nuevo directorio local.

---

## 🛠️ Resolución de Errores Comunes de Flujo

### Mensaje: `src refspec main does not match any`
* **Causa:** El comando push apunta a la rama `main` pero la rama local retiene el nombre `master`, o no se ha efectuado el commit inicial.
* **Solución:**
  ```bash
  git branch -M main
  git add .
  git commit -m "Commit inicial"
  git push -u origin main
  ```

### Mensaje: `fatal: remote origin already exists`
* **Causa:** El directorio local ya cuenta con un servidor remoto indexado bajo el nombre `origin`.
* **Solución:** Valide el destino actual mediante `git remote -v` y actualice la dirección utilizando `git remote set-url origin <URL_CORRECTA>`.
