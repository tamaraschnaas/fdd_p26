# Scripting Básico

![Scripting - Frieren automatizando hechizos](./images/06_scripting_frieren_automation.png)

Ahora que dominas variables, I/O y expansiones en la terminal, es hora de **guardar todo en archivos**: los scripts.

---

## ¿Qué es un Script?

Un script es un **archivo de texto con comandos** que se ejecutan en secuencia:

```bash
# En lugar de escribir esto cada vez:
echo "Hola"
date
whoami

# Lo guardas en un archivo y lo ejecutas cuando quieras
```

---

## Tu Primer Script

### Paso 1: Crear el archivo

```bash
# Crea el archivo con nano o cualquier editor
nano mi_primer_script.sh
```

### Paso 2: Agregar el shebang y comandos

```bash
#!/bin/bash
# Mi primer script

echo "¡Hola desde mi script!"
echo "Fecha: $(date)"
echo "Usuario: $USER"
echo "Directorio: $PWD"
```

### Paso 3: Hacerlo ejecutable

```bash
# Ver permisos actuales
ls -l mi_primer_script.sh
# -rw-r--r-- ... (no tiene permiso de ejecución)

# Dar permiso de ejecución
chmod +x mi_primer_script.sh

# Verificar
ls -l mi_primer_script.sh
# -rwxr-xr-x ... (ahora tiene la x)
```

### Paso 4: Ejecutar

```bash
# Forma 1: Con ./
./mi_primer_script.sh

# Forma 2: Con bash explícito
bash mi_primer_script.sh
```

:::exercise{title="Crear tu primer script" difficulty="1"}

```bash
# 1. Crea el archivo
cat << 'EOF' > hola.sh
#!/bin/bash
echo "==========================="
echo "  INFORMACIÓN DEL SISTEMA"
echo "==========================="
echo "Usuario: $USER"
echo "Fecha: $(date)"
echo "Directorio: $(pwd)"
echo "Shell: $SHELL"
echo "==========================="
EOF

# 2. Hazlo ejecutable
chmod +x hola.sh

# 3. Ejecútalo
./hola.sh
```

:::

---

## El Shebang: `#!/bin/bash`

### ¿Qué es el shebang?

El **shebang** (también llamado hashbang) es la primera línea de un script que le dice al sistema **qué programa debe interpretar el archivo**.

```
#!/bin/bash
││ └──────── Ruta al intérprete (el programa que ejecutará el script)
│└── ! (bang)
└── # (hash/sharp)

hash + bang = shebang
```

### ¿Cómo funciona?

Cuando ejecutas `./mi_script.sh`, el sistema operativo:

1. **Lee los primeros bytes** del archivo
2. **Busca `#!`** al inicio
3. **Lee la ruta** que sigue (ej: `/bin/bash`)
4. **Ejecuta ese programa** pasándole el script como argumento

```
Tu comando:        ./mi_script.sh
Lo que hace Linux: /bin/bash ./mi_script.sh
```

Es como si el shebang dijera: "Para leer este archivo, usa el programa `/bin/bash`".

### ¿Qué pasa SIN shebang?

Si tu script no tiene shebang y lo ejecutas con `./script.sh`:

```bash
# Script sin shebang
echo "Hola"
date
```

**Resultado:** El sistema intenta ejecutarlo con el shell por defecto (generalmente `/bin/sh`). Puede funcionar para comandos simples, pero:

- Algunas características de Bash no funcionarán en `/bin/sh`
- El comportamiento puede variar entre sistemas
- Es impredecible - **mala práctica**

**Solución:** Siempre incluir shebang.

### ¿Qué pasa si la ruta es incorrecta?

```bash
#!/bin/programa_que_no_existe
echo "Hola"
```

```bash
./script.sh
# bash: ./script.sh: /bin/programa_que_no_existe: bad interpreter: No such file or directory
```

El sistema no puede encontrar el intérprete → **error**.

### Shebangs comunes

| Shebang | Usa | Cuándo usarlo |
|---------|-----|---------------|
| `#!/bin/bash` | Bash directamente | Cuando sabes que Bash está en `/bin/bash` |
| `#!/bin/sh` | Shell POSIX básico | Scripts muy simples y portables |
| `#!/usr/bin/env bash` | Busca Bash en PATH | **Recomendado** - más portable |
| `#!/usr/bin/env python3` | Busca Python3 | Scripts de Python |

### ¿Por qué `/usr/bin/env`?

El problema con `#!/bin/bash`:
- En Linux, Bash suele estar en `/bin/bash`
- En macOS, puede estar en `/usr/local/bin/bash` (si instalaste con Homebrew)
- En otros sistemas, puede variar

**`/usr/bin/env bash`** resuelve esto:

```
#!/usr/bin/env bash
       │        │
       │        └── "busca un programa llamado bash"
       └── "usa el comando env para..."
```

`env` busca `bash` en tu `$PATH` y lo ejecuta. Funciona sin importar dónde esté instalado.

### Dos formas de ejecutar scripts

| Método | Necesita shebang | Necesita `chmod +x` |
|--------|------------------|---------------------|
| `./script.sh` | Sí | Sí |
| `bash script.sh` | No | No |

```bash
# Forma 1: Ejecutar directamente (usa el shebang)
chmod +x script.sh
./script.sh

# Forma 2: Llamar al intérprete explícitamente (ignora el shebang)
bash script.sh
```

:::exercise{title="Experimentar con el shebang" difficulty="2"}

```bash
# 1. Script con shebang correcto
cat << 'EOF' > test1.sh
#!/bin/bash
echo "Shell: $BASH_VERSION"
EOF
chmod +x test1.sh
./test1.sh

# 2. Script SIN shebang
cat << 'EOF' > test2.sh
echo "Sin shebang"
echo "Shell: $BASH_VERSION"
EOF
chmod +x test2.sh
./test2.sh
# ¿Funciona? ¿Muestra BASH_VERSION?

# 3. Script con shebang incorrecto
cat << 'EOF' > test3.sh
#!/bin/programa_falso
echo "Esto no se ejecutará"
EOF
chmod +x test3.sh
./test3.sh
# ¿Qué error da?

# 4. Usar env (portable)
cat << 'EOF' > test4.sh
#!/usr/bin/env bash
echo "Bash encontrado en: $(which bash)"
EOF
chmod +x test4.sh
./test4.sh

# Limpia
rm test1.sh test2.sh test3.sh test4.sh
```

:::

---

## Argumentos del Script

Los scripts pueden recibir argumentos de la línea de comandos:

```bash
./mi_script.sh argumento1 argumento2 argumento3
```

### Variables especiales de argumentos

| Variable | Contenido |
|----------|-----------|
| `$0` | Nombre del script |
| `$1` | Primer argumento |
| `$2` | Segundo argumento |
| `$n` | N-ésimo argumento |
| `$#` | Número de argumentos |
| `$@` | Todos los argumentos (como lista) |
| `$*` | Todos los argumentos (como string) |

```bash
#!/bin/bash
# argumentos.sh

echo "Script: $0"
echo "Primer argumento: $1"
echo "Segundo argumento: $2"
echo "Número de argumentos: $#"
echo "Todos: $@"
```

```bash
chmod +x argumentos.sh
./argumentos.sh hola mundo 123
# Script: ./argumentos.sh
# Primer argumento: hola
# Segundo argumento: mundo
# Número de argumentos: 3
# Todos: hola mundo 123
```

:::exercise{title="Script con argumentos" difficulty="2"}

```bash
# Crea saludar.sh
cat << 'EOF' > saludar.sh
#!/bin/bash
# Uso: ./saludar.sh nombre

if [ -z "$1" ]; then
    echo "Uso: $0 nombre"
    exit 1
fi

echo "¡Hola, $1!"
echo "Bienvenido al sistema."
EOF

chmod +x saludar.sh

# Prueba
./saludar.sh
./saludar.sh Juan
./saludar.sh "María García"
```

:::

---

## Códigos de Salida

Cada comando termina con un **código de salida**:

| Código | Significado |
|--------|-------------|
| `0` | Éxito |
| `1-255` | Error (el número indica el tipo) |

```bash
# Ver código del último comando
ls /tmp
echo $?  # 0 (éxito)

ls /directorio_falso
echo $?  # 2 (error)
```

### En scripts

```bash
#!/bin/bash

# Salir con éxito
exit 0

# Salir con error
exit 1
```

:::exercise{title="Códigos de salida" difficulty="1"}

```bash
# Ejecuta y observa $?
true
echo $?   # 0

false
echo $?   # 1

ls /tmp
echo $?   # 0

ls /noexiste
echo $?   # 2
```

:::

---

## Condicionales: `if`

### Sintaxis básica

```bash
if [ condición ]; then
    # código si es verdadero
fi
```

### Con else

```bash
if [ condición ]; then
    # código si verdadero
else
    # código si falso
fi
```

### Con elif

```bash
if [ condición1 ]; then
    # código
elif [ condición2 ]; then
    # código
else
    # código
fi
```

### Ejemplo

```bash
#!/bin/bash
edad=$1

if [ -z "$edad" ]; then
    echo "Uso: $0 edad"
    exit 1
fi

if [ $edad -ge 18 ]; then
    echo "Eres mayor de edad"
else
    echo "Eres menor de edad"
fi
```

---

## Condiciones de Test: `[ ]` y `[[ ]]`

### Comparación de números

| Operador | Significado |
|----------|-------------|
| `-eq` | Igual (equal) |
| `-ne` | No igual (not equal) |
| `-lt` | Menor que (less than) |
| `-le` | Menor o igual (less or equal) |
| `-gt` | Mayor que (greater than) |
| `-ge` | Mayor o igual (greater or equal) |

```bash
a=5
b=10

[ $a -eq $b ]  # falso
[ $a -lt $b ]  # verdadero
[ $a -ne $b ]  # verdadero
```

### Comparación de strings

| Operador | Significado |
|----------|-------------|
| `=` | Iguales |
| `!=` | Diferentes |
| `-z` | Vacío (zero length) |
| `-n` | No vacío (non-zero) |

```bash
nombre="Juan"

[ "$nombre" = "Juan" ]   # verdadero
[ "$nombre" != "Pedro" ] # verdadero
[ -z "$nombre" ]         # falso (no está vacío)
[ -n "$nombre" ]         # verdadero (no está vacío)
```

### Test de archivos

| Operador | Significado |
|----------|-------------|
| `-f` | Es un archivo regular |
| `-d` | Es un directorio |
| `-e` | Existe |
| `-r` | Es legible (readable) |
| `-w` | Es escribible (writable) |
| `-x` | Es ejecutable |

```bash
[ -f "/etc/passwd" ]  # verdadero
[ -d "/home" ]        # verdadero
[ -e "/tmp" ]         # verdadero
[ -x "./script.sh" ]  # depende de los permisos
```

:::exercise{title="Practicar condiciones" difficulty="2"}

```bash
# Crea verificar.sh
cat << 'EOF' > verificar.sh
#!/bin/bash
archivo=$1

if [ -z "$archivo" ]; then
    echo "Uso: $0 archivo"
    exit 1
fi

if [ -e "$archivo" ]; then
    echo "$archivo existe"
    
    if [ -f "$archivo" ]; then
        echo "  Es un archivo"
    elif [ -d "$archivo" ]; then
        echo "  Es un directorio"
    fi
    
    if [ -r "$archivo" ]; then
        echo "  Es legible"
    fi
    
    if [ -w "$archivo" ]; then
        echo "  Es escribible"
    fi
else
    echo "$archivo NO existe"
fi
EOF

chmod +x verificar.sh

# Prueba
./verificar.sh /etc/passwd
./verificar.sh /home
./verificar.sh /archivo_falso
```

:::

---

## Loops: `for` y `while`

### Loop `for`

```bash
# Iterar sobre una lista
for fruta in manzana naranja pera; do
    echo "Fruta: $fruta"
done

# Iterar sobre archivos
for archivo in *.txt; do
    echo "Archivo: $archivo"
done

# Iterar sobre números
for i in {1..5}; do
    echo "Número: $i"
done

# Estilo C
for ((i=1; i<=5; i++)); do
    echo "Número: $i"
done
```

### Loop `while`

```bash
# Mientras la condición sea verdadera
contador=1
while [ $contador -le 5 ]; do
    echo "Contador: $contador"
    contador=$((contador + 1))
done
```

:::exercise{title="Loops en acción" difficulty="2"}

```bash
# Crea contador.sh
cat << 'EOF' > contador.sh
#!/bin/bash
# Cuenta desde 1 hasta el argumento

limite=${1:-10}

echo "Contando hasta $limite:"
for i in $(seq 1 $limite); do
    echo $i
done
echo "¡Terminado!"
EOF

chmod +x contador.sh
./contador.sh 5
./contador.sh
```

:::

---

## Script Completo de Ejemplo

Aquí un script que combina todo lo aprendido:

```bash
#!/bin/bash
#
# backup.sh - Script de backup simple
# Uso: ./backup.sh [directorio_origen] [directorio_destino]
#

# Valores por defecto
ORIGEN=${1:-$HOME}
DESTINO=${2:-/tmp/backups}
FECHA=$(date +%Y%m%d_%H%M%S)
NOMBRE_BACKUP="backup_$(whoami)_$FECHA"

# Función para mostrar uso
mostrar_uso() {
    echo "Uso: $0 [origen] [destino]"
    echo "  origen:  Directorio a respaldar (default: \$HOME)"
    echo "  destino: Dónde guardar backup (default: /tmp/backups)"
}

# Verificar que origen existe
if [ ! -d "$ORIGEN" ]; then
    echo "Error: El directorio origen '$ORIGEN' no existe"
    mostrar_uso
    exit 1
fi

# Crear directorio destino si no existe
if [ ! -d "$DESTINO" ]; then
    echo "Creando directorio destino: $DESTINO"
    mkdir -p "$DESTINO"
fi

# Mostrar información
echo "==================================="
echo "  BACKUP SCRIPT"
echo "==================================="
echo "Origen:  $ORIGEN"
echo "Destino: $DESTINO/$NOMBRE_BACKUP.tar.gz"
echo "Fecha:   $(date)"
echo "==================================="

# Confirmar
read -p "¿Continuar? (s/n): " respuesta
if [ "$respuesta" != "s" ]; then
    echo "Backup cancelado."
    exit 0
fi

# Crear backup
echo "Creando backup..."
tar -czf "$DESTINO/$NOMBRE_BACKUP.tar.gz" -C "$(dirname $ORIGEN)" "$(basename $ORIGEN)" 2>/dev/null

# Verificar resultado
if [ $? -eq 0 ]; then
    tamaño=$(ls -lh "$DESTINO/$NOMBRE_BACKUP.tar.gz" | awk '{print $5}')
    echo "¡Backup completado!"
    echo "Archivo: $DESTINO/$NOMBRE_BACKUP.tar.gz"
    echo "Tamaño: $tamaño"
    exit 0
else
    echo "Error al crear el backup"
    exit 1
fi
```

:::exercise{title="Probar el script de backup" difficulty="3"}

```bash
# 1. Crea el script (copia el código de arriba)
nano backup.sh

# 2. Hazlo ejecutable
chmod +x backup.sh

# 3. Prueba (con valores por defecto)
./backup.sh

# 4. Prueba con argumentos
mkdir -p ~/test_backup
echo "archivo de prueba" > ~/test_backup/prueba.txt
./backup.sh ~/test_backup /tmp/mis_backups

# 5. Verifica el backup
ls -la /tmp/mis_backups/
```

:::

---

## Debugging de Scripts

### Modo verbose: `bash -v`

```bash
bash -v mi_script.sh
# Muestra cada línea antes de ejecutarla
```

### Modo trace: `bash -x`

```bash
bash -x mi_script.sh
# Muestra cada comando expandido
```

### Dentro del script

```bash
#!/bin/bash
set -x  # Activar trace
# ... código ...
set +x  # Desactivar trace
```

---

## Buenas Prácticas

1. **Siempre incluir shebang**: `#!/bin/bash`
2. **Comentar el código**: explicar qué hace
3. **Validar argumentos**: verificar que existen y son válidos
4. **Usar valores por defecto**: `${VAR:-default}`
5. **Verificar códigos de salida**: `$?`
6. **Citar variables**: `"$variable"` para evitar problemas con espacios
7. **Usar `set -e`**: terminar si un comando falla

```bash
#!/bin/bash
set -e  # Salir si hay error
set -u  # Error si variable no definida
```

---

## Resumen

| Concepto | Sintaxis |
|----------|----------|
| Shebang | `#!/bin/bash` |
| Ejecutar | `chmod +x script.sh && ./script.sh` |
| Argumentos | `$1`, `$2`, `$@`, `$#` |
| Código salida | `$?`, `exit 0`, `exit 1` |
| Condicional | `if [ condición ]; then ... fi` |
| Loop for | `for i in lista; do ... done` |
| Loop while | `while [ cond ]; do ... done` |
| Test archivo | `-f`, `-d`, `-e`, `-r`, `-w` |
| Test número | `-eq`, `-lt`, `-gt`, `-ne` |
| Test string | `=`, `!=`, `-z`, `-n` |

---

## ¿Qué sigue?

Has aprendido los fundamentos de Bash como lenguaje:
- Variables y su sintaxis
- Variables de entorno
- Entrada/salida
- Expansiones y sustituciones
- Scripting básico

Ahora puedes:
- Automatizar tareas repetitivas
- Crear herramientas personalizadas
- Entender scripts que encuentres
- Continuar aprendiendo temas avanzados (arrays, funciones complejas, regex)

---

> **¡Felicidades!** Has completado el módulo de Bash.
