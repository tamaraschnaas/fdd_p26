# Variables en Bash

![Variables - Frieren sacando objetos de su bolsa mágica](./images/02_variables_frieren_bag.png)

Las **variables** son contenedores para guardar información. Son fundamentales en cualquier lenguaje de programación, incluyendo Bash.

---

## ¿Qué es una Variable?

Una variable es un **nombre** que apunta a un **valor**:

```
┌─────────────┐
│  nombre     │ ──────► "Juan"
└─────────────┘

┌─────────────┐
│  edad       │ ──────► 25
└─────────────┘

┌─────────────┐
│  directorio │ ──────► "/home/usuario"
└─────────────┘
```

---

## Crear Variables (Asignación)

### Sintaxis: `NOMBRE=valor`

```bash
# Texto
nombre="Juan"
mensaje="Hola mundo"

# Números
edad=25
contador=0

# Rutas
directorio="/home/usuario/proyectos"
```

### REGLA CRÍTICA: Sin Espacios

```bash
# CORRECTO - sin espacios alrededor del =
nombre="Juan"

# INCORRECTO - Bash interpreta "nombre" como un comando
nombre = "Juan"
# Error: nombre: command not found

nombre= "Juan"
# Error

nombre ="Juan"
# Error
```

:::exercise{title="Crear tus primeras variables" difficulty="1"}

Ejecuta en tu terminal:

```bash
# Crea estas variables
mi_nombre="Tu Nombre Aquí"
mi_edad=20
mi_lenguaje="Python"

# Verifica que existen (no producen output, eso es normal)
```

:::

---

## Leer Variables: `$VAR` y `${VAR}`

Para **usar** el valor de una variable, necesitas el símbolo `$`:

```bash
nombre="Juan"

# Leer la variable
echo $nombre
# Juan

echo "Hola, $nombre"
# Hola, Juan
```

### `$VAR` vs `${VAR}` - ¿Cuál usar?

Ambos funcionan, pero `${VAR}` es más seguro:

```bash
archivo="documento"

# $VAR funciona aquí
echo $archivo
# documento

# Pero falla si quieres agregar texto
echo "$archivo_backup"
# (vacío - Bash busca variable "archivo_backup")

# ${VAR} resuelve el problema
echo "${archivo}_backup"
# documento_backup
```

### Regla Simple

| Situación | Usar | Ejemplo |
|-----------|------|---------|
| Variable sola | `$VAR` | `echo $nombre` |
| Variable + texto | `${VAR}` | `echo "${nombre}_copia"` |
| Dentro de comillas | `${VAR}` | `echo "Hola ${nombre}"` |
| En duda | `${VAR}` | Siempre funciona |

:::exercise{title="Practicar $VAR vs ${VAR}" difficulty="2"}

```bash
# Crea una variable
fruta="manzana"

# Prueba estas variaciones
echo $fruta
echo ${fruta}
echo "${fruta}s"         # manzanas
echo "$frutas"           # ¿Qué pasa? (vacío)
echo "${fruta}s"         # manzanas (correcto)

# Más ejemplos
prefijo="archivo"
echo "${prefijo}_001.txt"
echo "${prefijo}_002.txt"
```

:::

---

## Comillas: Simples vs Dobles (MUY IMPORTANTE)

Esta es una de las fuentes de errores más comunes en Bash.

### Comillas Dobles `"..."` - Variables SÍ se expanden

```bash
nombre="María"

echo "Hola, $nombre"
# Hola, María

echo "Tu home es $HOME"
# Tu home es /home/usuario
```

### Comillas Simples `'...'` - Variables NO se expanden

```bash
nombre="María"

echo 'Hola, $nombre'
# Hola, $nombre  (literal, no expande)

echo 'Tu home es $HOME'
# Tu home es $HOME  (literal)
```

### Tabla Comparativa

| Comillas | Variables | Caracteres especiales | Usar para |
|----------|-----------|----------------------|-----------|
| `"dobles"` | Se expanden | Algunos se interpretan | Texto con variables |
| `'simples'` | NO se expanden | Se imprimen literal | Texto literal exacto |
| Sin comillas | Se expanden | Se interpretan | Variables simples |

:::exercise{title="Comillas simples vs dobles" difficulty="2"}

```bash
nombre="Carlos"
ruta="/home/$nombre"

# Predice el resultado antes de ejecutar
echo "Dobles: $nombre"
echo 'Simples: $nombre'

echo "Ruta con dobles: $ruta"
echo 'Ruta con simples: $ruta'

# ¿Cuál es la diferencia?
```

:::

---

## Variables Especiales de Bash

Bash tiene variables predefinidas con información útil:

| Variable | Contenido |
|----------|-----------|
| `$USER` | Tu nombre de usuario |
| `$HOME` | Tu directorio home |
| `$PWD` | Directorio actual |
| `$SHELL` | Tu shell |
| `$?` | Código de salida del último comando |
| `$$` | PID del proceso actual |

```bash
echo "Usuario: $USER"
echo "Home: $HOME"
echo "Directorio actual: $PWD"
echo "Mi shell: $SHELL"
```

:::exercise{title="Explorar variables especiales" difficulty="1"}

```bash
# Ejecuta cada línea y observa
echo "Soy $USER"
echo "Mi home está en $HOME"
echo "Estoy en $PWD"
echo "Mi proceso es $$"

# El código de salida
ls /tmp
echo "Código de salida: $?"

ls /directorio_que_no_existe
echo "Código de salida: $?"
# ¿Qué diferencia hay?
```

:::

---

## Concatenar Variables

Puedes combinar variables y texto:

```bash
nombre="Juan"
apellido="Pérez"

# Concatenar
nombre_completo="$nombre $apellido"
echo $nombre_completo
# Juan Pérez

# Con más texto
saludo="Hola, $nombre_completo, bienvenido"
echo $saludo
# Hola, Juan Pérez, bienvenido

# Construir rutas
usuario="maria"
ruta_proyecto="/home/${usuario}/proyectos/nuevo"
echo $ruta_proyecto
# /home/maria/proyectos/nuevo
```

:::exercise{title="Construir mensajes con variables" difficulty="2"}

```bash
# Define variables
nombre="Ada"
lenguaje="Python"
años_exp=5

# Construye un mensaje (usa estas variables)
mensaje="$nombre programa en $lenguaje y tiene $años_exp años de experiencia"
echo $mensaje

# Construye una ruta
base="/home"
usuario="ada"
proyecto="ml_model"
ruta_completa="${base}/${usuario}/proyectos/${proyecto}"
echo $ruta_completa
```

:::

---

## Eliminar Variables: `unset`

```bash
mi_variable="algo"
echo $mi_variable
# algo

unset mi_variable
echo $mi_variable
# (vacío)
```

---

## Ver Todas las Variables

```bash
# Ver todas las variables del shell
set | head -20

# Ver solo variables de entorno
env | head -20
```

---

## Errores Comunes

### 1. Espacios en la asignación

```bash
# MAL
nombre = "Juan"
# bash: nombre: command not found

# BIEN
nombre="Juan"
```

### 2. Olvidar el $

```bash
nombre="Juan"

# MAL - imprime la palabra "nombre"
echo nombre
# nombre

# BIEN
echo $nombre
# Juan
```

### 3. Confundir comillas

```bash
var="mundo"

# Comillas dobles - expande
echo "Hola $var"   # Hola mundo

# Comillas simples - NO expande
echo 'Hola $var'   # Hola $var
```

### 4. Variable pegada a texto

```bash
archivo="foto"

# MAL - busca variable "archivobak"
echo "$archivobak"
# (vacío)

# BIEN - usa llaves
echo "${archivo}bak"
# fotobak
```

---

## Ejercicios Integrales

:::exercise{title="Crear tarjeta de presentación" difficulty="2"}

Crea variables con tu información y genera una "tarjeta":

```bash
# Tu información
nombre="Tu Nombre"
ocupacion="Estudiante"
universidad="ITAM"
semestre=5
hobby="programar"

# Genera la tarjeta
echo "================================"
echo "  TARJETA DE PRESENTACIÓN"
echo "================================"
echo "Nombre: $nombre"
echo "Ocupación: $ocupacion"
echo "Universidad: $universidad"
echo "Semestre: $semestre"
echo "Hobby: $hobby"
echo "================================"
```

:::

:::exercise{title="Construir rutas dinámicas" difficulty="2"}

```bash
# Variables base
usuario=$(whoami)
fecha=$(date +%Y-%m-%d)
proyecto="analisis"

# Construye rutas
ruta_home="/home/${usuario}"
ruta_backup="${ruta_home}/backups/${fecha}"
ruta_proyecto="${ruta_home}/proyectos/${proyecto}"

# Muestra las rutas
echo "Home: $ruta_home"
echo "Backup: $ruta_backup"
echo "Proyecto: $ruta_proyecto"
```

:::

:::exercise{title="Variables y cálculos" difficulty="3"}

```bash
# Datos
precio_unitario=150
cantidad=5
descuento=10

# Cálculos
subtotal=$((precio_unitario * cantidad))
descuento_valor=$((subtotal * descuento / 100))
total=$((subtotal - descuento_valor))

# Factura
echo "=== FACTURA ==="
echo "Precio unitario: \$$precio_unitario"
echo "Cantidad: $cantidad"
echo "Subtotal: \$$subtotal"
echo "Descuento ($descuento%): -\$$descuento_valor"
echo "TOTAL: \$$total"
```

:::

---

## Resumen

| Concepto | Sintaxis | Ejemplo |
|----------|----------|---------|
| Crear variable | `VAR=valor` | `nombre="Juan"` |
| Leer variable | `$VAR` o `${VAR}` | `echo $nombre` |
| Con texto | `${VAR}texto` | `echo "${nombre}_backup"` |
| Comillas dobles | `"$VAR"` | Expande variables |
| Comillas simples | `'$VAR'` | NO expande (literal) |
| Eliminar | `unset VAR` | `unset nombre` |

---

> **Siguiente:** Ahora veremos las **variables de entorno** - variables especiales que configuran tu sistema.
