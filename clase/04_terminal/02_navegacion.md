# Navegación y Rutas

![Navegación - Frieren y grupo viajando por un mapa de árbol de directorios](./images/02_navigation_frieren_journey.png)

:::exercise{title="Quiz de rutas (práctica para Bandit)" difficulty="2"}

Responde mentalmente estas preguntas (te servirán en Bandit):

1. Si estás en `/home/usuario/proyectos/python`, ¿cuál es la ruta relativa a `/home/usuario/documentos`?
2. ¿Qué significa `~` y a qué ruta absoluta equivale en tu sistema?
3. Si ejecutas `cd ../..` desde `/home/usuario/a/b/c`, ¿en qué directorio terminas?
4. ¿Cuál es la diferencia entre `cd /home` y `cd home`?
5. ¿Qué comando usas para volver al directorio donde estabas antes?

**Tip:** Estos conceptos aparecen en los niveles de Bandit.

:::

Entender el sistema de archivos y cómo moverte es fundamental. Esta sección cubre uno de los conceptos más importantes: **rutas**.

---

![Sistema de archivos - Árbol de Yggdrasil estilo Serial Experiments Lain](./images/02_filesystem_lain_tree.png)

## El Sistema de Archivos

En Unix (Linux/macOS), todo es un árbol que empieza en `/` (raíz):

```
/                         ← Raíz (root)
├── home/                 ← Carpetas de usuarios
│   ├── maria/            ← Home de maria
│   │   ├── Documents/
│   │   ├── Downloads/
│   │   └── projects/
│   └── juan/             ← Home de juan
├── usr/                  ← Programas del sistema
│   ├── bin/              ← Comandos/ejecutables
│   └── lib/              ← Librerías
├── etc/                  ← Configuración del sistema
├── tmp/                  ← Archivos temporales
└── var/                  ← Datos variables (logs, etc.)
```

### Directorios Principales de Unix/Linux

En Unix/Linux, cada directorio tiene un propósito específico. Esto viene de décadas de convención:

| Directorio | Nombre | ¿Qué contiene? |
|------------|--------|----------------|
| `/` | **Root** (raíz) | El directorio padre de TODO. Es el inicio del árbol. |
| `/home` | **Home** | Carpetas personales de cada usuario (`/home/maria`, `/home/juan`) |
| `/root` | **Root user home** | Carpeta personal del administrador (superusuario) |
| `/bin` | **Binaries** | Comandos esenciales (`ls`, `cp`, `mv`, `cat`) |
| `/sbin` | **System binaries** | Comandos de administración (`shutdown`, `mount`) |
| `/usr` | **User programs** | Programas instalados por el usuario/sistema |
| `/usr/bin` | — | Más comandos y programas |
| `/usr/local` | — | Software instalado manualmente |
| `/etc` | **Et cetera / Config** | Archivos de configuración del sistema |
| `/var` | **Variable** | Datos que cambian: logs, bases de datos, caché |
| `/var/log` | — | Archivos de registro (logs) |
| `/tmp` | **Temporary** | Archivos temporales (se borran al reiniciar) |
| `/dev` | **Devices** | Archivos especiales que representan hardware |
| `/mnt` | **Mount** | Punto de montaje para discos externos |
| `/opt` | **Optional** | Software opcional de terceros |

**¿Por qué importa esto?**

- Cuando busques **configuraciones** → mira en `/etc`
- Cuando busques **logs de errores** → mira en `/var/log`
- Cuando quieras **guardar tus cosas** → usa tu `/home/usuario`
- Los **comandos** están en `/bin` y `/usr/bin`

### El Usuario Root vs Tu Usuario

| Concepto | Descripción |
|----------|-------------|
| **root** (usuario) | El superusuario/administrador. Puede hacer TODO. |
| `/root` | La carpeta home del usuario root |
| `/` (root directory) | El directorio raíz del sistema (diferente al usuario) |
| **tu usuario** | Usuario normal con permisos limitados |
| `/home/tu_usuario` | Tu carpeta personal (también llamada `~`) |

```bash
# Ver quién eres
whoami
# maria (usuario normal)

# Tu home
echo $HOME
# /home/maria

# El home de root (necesitas permisos)
ls /root
# ls: cannot open directory '/root': Permission denied
```

> **Nota:** En Linux casi nunca trabajas como root directamente. Usas `sudo` para ejecutar comandos específicos con permisos de administrador.

### Diferencias con Windows

| Unix/Linux/macOS | Windows |
|------------------|---------|
| `/` | `C:\` |
| `/home/usuario` | `C:\Users\usuario` |
| `/` separador | `\` separador |
| Case sensitive: `Archivo` ≠ `archivo` | Case insensitive |

---

![Rutas - Dos caminos: uno desde el origen (absoluto) y otro desde donde estás (relativo)](./images/02_paths_evangelion_nerv.png)

## Rutas Absolutas vs Relativas

Este es el concepto **más importante** de esta sección.

### Ruta Absoluta

Empieza desde la raíz (`/`). Es la dirección completa.

```bash
/home/maria/Documents/proyecto/archivo.txt
```

- Siempre empieza con `/`
- Funciona desde cualquier ubicación
- Es como dar la dirección completa de una casa

### Ruta Relativa

Empieza desde donde estás. Es relativa a tu ubicación actual.

```bash
# Si estás en /home/maria/
Documents/proyecto/archivo.txt

# Si estás en /home/maria/Documents/
proyecto/archivo.txt
```

- **No** empieza con `/`
- Depende de dónde estés
- Es como decir "a dos cuadras a la derecha"

### Símbolos Especiales para Rutas

| Símbolo | Significado | Ejemplo |
|---------|-------------|---------|
| `/` | Raíz del sistema | `cd /` |
| `~` | Tu directorio home | `cd ~` = `cd /home/tu_usuario` |
| `.` | Directorio actual | `./script.sh` |
| `..` | Directorio padre (uno arriba) | `cd ..` |
| `-` | Directorio anterior | `cd -` |

### Ejemplos Prácticos

```bash
# Estás en: /home/maria/Documents/proyecto

pwd
# /home/maria/Documents/proyecto

# Ir al home (3 formas equivalentes)
cd ~
cd /home/maria
cd

# Ir un nivel arriba (a Documents)
cd ..

# Ir dos niveles arriba (a home/maria)
cd ../..

# Ir a una carpeta hermana
cd ../otra_carpeta

# Volver al directorio anterior
cd -
```

---

## Comandos de Navegación

### `pwd` - Dónde estoy

**P**rint **W**orking **D**irectory

```bash
pwd
# /home/maria/Documents
```

### `ls` - Qué hay aquí

**L**i**s**t - lista el contenido de un directorio.

```bash
# Listar directorio actual
ls

# Listar otro directorio
ls /home

# Listar con detalles
ls -l

# Listar incluyendo archivos ocultos
ls -a

# Listar con detalles + ocultos + tamaños legibles
ls -lah
```

#### Entendiendo `ls -l` (formato largo)

La bandera `-l` muestra información **detallada** de cada archivo. Ejemplo:

```
drwxr-xr-x  2 maria maria 4096 Jan 20 10:30 Documents
-rw-r--r--  1 maria maria  256 Jan 19 09:15 notas.txt
```

**Desglose columna por columna:**

```
d rwxr-xr-x  2  maria  maria  4096  Jan 20 10:30  Documents
│ │          │  │      │      │     │             │
│ │          │  │      │      │     │             └── Nombre del archivo/carpeta
│ │          │  │      │      │     └── Fecha de última modificación
│ │          │  │      │      └── Tamaño en bytes
│ │          │  │      └── Grupo dueño
│ │          │  └── Usuario dueño
│ │          └── Número de enlaces (hard links)
│ └── Permisos (rwxr-xr-x)
└── Tipo: d=directorio, -=archivo, l=link simbólico
```

**Tabla resumen:**

| Columna | Ejemplo | Significado |
|---------|---------|-------------|
| 1 | `d` o `-` | **Tipo:** `d`=directorio, `-`=archivo, `l`=link |
| 2 | `rwxr-xr-x` | **Permisos** (ver abajo) |
| 3 | `2` | Número de enlaces duros |
| 4 | `maria` | **Usuario** dueño del archivo |
| 5 | `maria` | **Grupo** dueño del archivo |
| 6 | `4096` | **Tamaño** en bytes |
| 7 | `Jan 20 10:30` | **Fecha** de última modificación |
| 8 | `Documents` | **Nombre** del archivo o carpeta |

#### Entendiendo los permisos (`rwxr-xr-x`)

Los permisos se dividen en 3 grupos de 3 caracteres:

```
rwx r-x r-x
│   │   │
│   │   └── Permisos para OTROS (everyone else)
│   └── Permisos para el GRUPO
└── Permisos para el DUEÑO (owner)
```

| Letra | Significado | En archivos | En directorios |
|-------|-------------|-------------|----------------|
| `r` | **R**ead (leer) | Ver contenido | Listar archivos (`ls`) |
| `w` | **W**rite (escribir) | Modificar | Crear/eliminar archivos |
| `x` | E**x**ecute (ejecutar) | Ejecutar como programa | Entrar al directorio (`cd`) |
| `-` | Sin permiso | — | — |

**Ejemplos de permisos:**

| Permisos | Significado |
|----------|-------------|
| `rwxr-xr-x` | Dueño: todo. Grupo y otros: leer y ejecutar |
| `rw-r--r--` | Dueño: leer/escribir. Otros: solo leer |
| `rwx------` | Solo el dueño puede hacer todo |
| `rwxrwxrwx` | Todos pueden hacer todo (¡peligroso!) |

#### Las banderas más útiles de `ls`

| Bandera | Significado | Ejemplo |
|---------|-------------|---------|
| `-l` | Formato **l**argo (detalles) | `ls -l` |
| `-a` | **A**ll - incluir archivos ocultos (empiezan con `.`) | `ls -a` |
| `-h` | **H**uman readable - tamaños en KB, MB, GB | `ls -lh` |
| `-t` | Ordenar por **t**iempo (más reciente primero) | `ls -lt` |
| `-S` | Ordenar por **S**ize (más grande primero) | `ls -lS` |
| `-r` | **R**everse - invertir orden | `ls -lr` |
| `-R` | **R**ecursivo - incluir subdirectorios | `ls -R` |

**Combinaciones útiles:**

```bash
ls -la      # Detalles + ocultos
ls -lah     # Detalles + ocultos + tamaños legibles
ls -lt      # Ordenados por fecha (recientes primero)
ls -lS      # Ordenados por tamaño (grandes primero)
ls -latr    # Todos, por fecha, más antiguo primero
```

:::exercise{title="Explorar ls -l" difficulty="1"}

1. Ejecuta `ls -l` en tu home y responde:
   - ¿Cuántos directorios hay? (empiezan con `d`)
   - ¿Cuál es el archivo más grande?
   - ¿Quién es el dueño de los archivos?

2. Compara:
   ```bash
   ls -l
   ls -la
   ```
   ¿Qué archivos nuevos aparecen con `-a`? ¿Con qué empiezan?

3. Compara:
   ```bash
   ls -l
   ls -lh
   ```
   ¿Cómo cambian los tamaños?

:::

### `cd` - Cambiar directorio

**C**hange **D**irectory

```bash
# Ir a una ruta absoluta
cd /home/maria/Documents

# Ir a una ruta relativa
cd Documents

# Ir al home
cd ~
cd     # Equivalente

# Ir un nivel arriba
cd ..

# Ir a la raíz
cd /

# Volver al directorio anterior
cd -
```

---

![Home directory - La acogedora habitación de Lain llena de computadoras](./images/02_home_lain_room.png)

## El Directorio Home (`~`)

Tu **home** es tu espacio personal. Es donde:
- Guardas tus archivos
- Están tus configuraciones (archivos que empiezan con `.`)
- Empiezas cuando abres la terminal

```bash
# Ver tu home
echo $HOME
# /home/tu_usuario

# Ir al home (todas equivalentes)
cd
cd ~
cd $HOME
cd /home/tu_usuario
```

### Archivos ocultos (dotfiles)

Los archivos que empiezan con `.` son ocultos:

```bash
ls -a ~
# .bashrc     ← Configuración de Bash
# .profile    ← Configuración de login
# .ssh/       ← Llaves SSH
# .gitconfig  ← Configuración de Git
```

---

## Rutas en WSL2

Si usas WSL2, puedes acceder a los archivos de Windows:

```bash
# Tus archivos de Windows están en:
/mnt/c/Users/TuUsuarioWindows/

# Ejemplo: ir a Documentos de Windows
cd /mnt/c/Users/Juan/Documents

# Tu home de Linux está en:
/home/tu_usuario_linux/
```

> **Recomendación:** Trabaja en tu home de Linux (`~`) para mejor rendimiento. Solo accede a `/mnt/c/` cuando necesites archivos específicos de Windows.

---

## Ejercicios Prácticos

:::exercise{title="Navegación básica" difficulty="1"}

1. Abre tu terminal
2. Ejecuta `pwd` - ¿dónde estás?
3. Ejecuta `ls` - ¿qué hay?
4. Ejecuta `cd /` y luego `ls` - ¿qué ves en la raíz?
5. Ejecuta `cd ~` para volver a tu home

:::

:::exercise{title="Rutas relativas" difficulty="2"}

Desde tu home (`~`):

1. Crea una estructura: `mkdir -p proyectos/python/ejercicio1`
2. Navega a `ejercicio1` usando ruta relativa
3. Ejecuta `pwd` para verificar
4. Vuelve a home usando `cd ../../../`
5. Verifica con `pwd`

:::

:::exercise{title="Diferencia absoluta vs relativa" difficulty="2"}

1. Navega a `/tmp` usando ruta absoluta
2. Desde `/tmp`, intenta ir a tu home de dos formas:
   - Ruta absoluta: `cd /home/tu_usuario`
   - Usando `~`: `cd ~`
3. ¿Cuál es más corta?

:::

---

## Resumen de Rutas

| Tipo | Ejemplo | Cuándo usar |
|------|---------|-------------|
| Absoluta | `/home/maria/docs` | Scripts, configuraciones |
| Relativa | `docs/archivo.txt` | Navegación rápida |
| Home | `~/projects` | Acceder a tus archivos |
| Padre | `../` | Subir niveles |
| Actual | `./script.sh` | Ejecutar archivos locales |

---

## Más Ejercicios

:::exercise{title="Mapa mental del sistema" difficulty="2"}

Explora estas rutas y anota qué contienen:

```bash
ls /
ls /home
ls /usr/bin | head -20
ls /etc | head -20
ls /tmp
```

**Pregunta:** ¿Para qué crees que sirve cada directorio?

:::

:::exercise{title="Carrera de navegación" difficulty="2"}

Cronometra cuánto tardas en:

1. Ir a `/var/log`
2. Volver a tu home
3. Ir a `/etc`
4. Volver al directorio anterior (`cd -`)
5. Crear `~/prueba/nivel1/nivel2/nivel3` con un solo comando

**Objetivo:** Hazlo en menos de 30 segundos.

:::

:::exercise{title="Encuentra tu archivo de configuración de bash" difficulty="2"}

1. Ve a tu home: `cd ~`
2. Lista archivos ocultos: `ls -la`
3. Encuentra `.bashrc` o `.zshrc`
4. Muestra su contenido: `cat ~/.bashrc`

**Pregunta:** ¿Qué tipo de configuraciones tiene?

:::

---

## Prompts para LLM

:::prompt{title="Convertir ruta Windows a Linux" for="ChatGPT/Claude"}

Tengo esta ruta de Windows:

```
C:\Users\MiUsuario\Documents\proyecto\archivo.txt
```

¿Cómo accedo a este archivo desde WSL2? Explícame cómo funcionan las rutas en /mnt/c/

:::

:::prompt{title="Entender estructura de directorios" for="ChatGPT/Claude"}

Soy nuevo en Linux. Explícame para qué sirven estos directorios del sistema:
- /home
- /etc
- /var
- /usr
- /tmp
- /bin

Dame ejemplos prácticos de cuándo usaría cada uno.

:::

:::prompt{title="Practicar rutas" for="ChatGPT/Claude"}

Dame 5 ejercicios de práctica para convertir rutas absolutas a relativas y viceversa en Linux. Incluye las respuestas para que pueda verificar.

Estoy en el directorio: /home/usuario/proyectos/python

:::

---

## Resumen de Comandos

| Comando | Descripción |
|---------|-------------|
| `pwd` | Muestra directorio actual |
| `ls` | Lista contenido |
| `ls -la` | Lista con detalles y ocultos |
| `cd ruta` | Cambia a directorio |
| `cd` o `cd ~` | Va al home |
| `cd ..` | Sube un nivel |
| `cd -` | Vuelve al anterior |
