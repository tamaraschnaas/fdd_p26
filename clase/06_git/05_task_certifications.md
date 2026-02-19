:::homework{id="6.1" title="Certificación GitHub Concepts + Configuración SSH" due="2026-01-29" points="20"}

Completa el curso [Introduction to GitHub Concepts](https://app.datacamp.com/learn/courses/introduction-to-github-concepts) y configura tu entorno SSH. Sube evidencia siguiendo las instrucciones de este documento.

**Entrega:**
1. Pull Request con tu carpeta y evidencias
2. En Canvas: Link al PR + Link a tu carpeta de evidencias

:::

# Tarea: Configuración y Certificación de GitHub

Este ejercicio es tu "examen de manejo" para el curso. Pondrás en práctica el flujo de trabajo con Git y GitHub.

**Objetivo:** Crear tu espacio de trabajo personal y entregar evidencia del curso completado mediante Pull Request + Canvas.

---

## Instrucciones Paso a Paso

Todo se hace **desde la terminal, manualmente**.

### 1. Preparación del Entorno (Fork & Clone)

**Paso 0: Fork**
Ve a `https://github.com/sonder-art/fdd_p26` y haz un **Fork** hacia tu cuenta personal.

Si aún no has clonado **TU FORK** y configurado el upstream, hazlo ahora (ver [02_repo_structure.md](./02_repo_structure.md)).

> **¡Alto!** Asegúrate de haber clonado `github.com/TU_USUARIO/...` y no `github.com/sonder-art/...`. Si clonaste el del profesor por error, no podrás subir tus cambios.

**Sincroniza tu repositorio:**

```bash
git checkout main
git pull upstream main
git push origin main
```

### 2. Crear la Rama de la Tarea

Crea una rama específica para esta entrega:

```bash
git checkout -b tarea-certificaciones
```

Verifica que estás en la rama correcta:

```bash
git status
# Debe decir: On branch tarea-certificaciones
```

### 3. Crear tu Espacio Personal

```bash
# Ve a la carpeta estudiantes
cd estudiantes

# Crea tu carpeta (usa tu usuario de GitHub exacto)
mkdir tu_usuario

# Crea un archivo para que Git detecte la carpeta
touch tu_usuario/.gitkeep

# Crea la carpeta para certificaciones
mkdir tu_usuario/certificaciones
```

### 4. Completar el Curso y Agregar Evidencia

1. **Completa el curso:** [Introduction to GitHub Concepts](https://app.datacamp.com/learn/courses/introduction-to-github-concepts)

2. **Toma captura de pantalla** donde se vea:
   - Tu nombre/usuario
   - El curso completado al 100%

3. **Guarda la imagen:**
   ```bash
   # Copia tu screenshot a la carpeta (ajusta el nombre/ruta de tu imagen)
   cp ~/Downloads/screenshot.png estudiantes/tu_usuario/certificaciones/evidencia_github.png
   ```

4. **Crea un archivo con la evidencia:**
   ```bash
   # Crea el archivo de evidencia
   cat << EOF > estudiantes/tu_usuario/certificaciones/github.md
   # Certificación GitHub Concepts

   **Estudiante:** Tu Nombre
   **Fecha:** $(date +%Y-%m-%d)

   ## Evidencia

   ![Certificado GitHub](./evidencia_github.png)
   EOF
   ```

### 5. Guardar y Subir (Commit & Push)

```bash
# Verifica qué archivos vas a agregar
git status

# Agrega solo tu carpeta
git add estudiantes/tu_usuario/

# Verifica que solo agregaste lo tuyo
git status

# Haz el commit
git commit -m "Agrego certificación de GitHub Concepts"

# Sube tu rama
git push origin tarea-certificaciones
```

### 6. Crear el Pull Request

1. Ve a la página de **TU Fork** en GitHub
2. Click en "Compare & pull request" (o ve a Pull Requests → New)
3. Configura:
   - **Base repository:** `sonder-art/fdd_p26` → `main`
   - **Head repository:** `tu-usuario/fdd_p26` → `tarea-certificaciones`
4. **Título:** `[Certificaciones] Tu Nombre`
5. **Descripción:**
   ```
   Entrego mi certificación de GitHub Concepts.
   - [x] Carpeta personal creada
   - [x] Curso completado
   - [x] Evidencia agregada
   ```
6. Click en **Create Pull Request**
7. **Copia la URL del PR** (la necesitas para Canvas)

### 7. Subir a Canvas (OBLIGATORIO)

**El PR no es suficiente.** Ve a la tarea correspondiente en Canvas y sube **DOS links**:

```
Pull Request: https://github.com/sonder-art/fdd_p26/pull/XX
Archivos: https://github.com/tu-usuario/fdd_p26/tree/tarea-certificaciones/estudiantes/tu_usuario/certificaciones
```

> **⚠️ Sin los dos links en Canvas, la tarea está incompleta.**

---

## Resumen de Entrega

| Qué | Dónde |
|-----|-------|
| Tu código/evidencias | Pull Request en GitHub |
| Link al PR | Canvas |
| Link a tu carpeta | Canvas |

**Los dos son obligatorios. PR + Canvas.**

---

## ¿Algo salió mal?

Si Git te da errores:

1. **No entres en pánico.** Es normal.
2. Copia el error completo de la terminal.
3. Busca el error en Google o pregunta a tu LLM con contexto:
   > "Estoy intentando hacer [lo que hacías]. Me salió este error:
   > [PEGAR ERROR]
   > ¿Cómo lo soluciono?"

---

## Tarea 2: Intermediate GitHub Concepts

Una vez que completes esta primera tarea, hay una segunda certificación:

**Curso:** [Intermediate GitHub Concepts](https://app.datacamp.com/learn/courses/intermediate-github-concepts)

**Entrega:** Exactamente igual que esta tarea:
1. Guarda tu evidencia en `estudiantes/tu_usuario/certificaciones/evidencia_github_intermediate.png`
2. Crea o actualiza el archivo `github_intermediate.md` con la imagen
3. Haz commit, push y Pull Request
4. Sube los dos links a Canvas (PR + carpeta)

Puedes usar la misma rama o crear una nueva (`tarea-certificaciones-2`).

