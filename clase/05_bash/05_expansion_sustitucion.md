# Expansión y Sustitución

![Expansión - Frieren transformando y expandiendo hechizos](./images/05_expansion_frieren_magic.png)

La **expansión** es cuando Bash transforma algo que escribes en otra cosa antes de ejecutar el comando. Es el corazón del poder de Bash.

---

## La Familia del `$` - Mapa Completo

{% raw %}
| Sintaxis | Nombre | Qué hace | Ejemplo |
|----------|--------|----------|---------|
| `$VAR` | Variable | Lee valor de variable | `echo $HOME` |
| `${VAR}` | Variable (segura) | Lee valor, permite modificadores | `echo ${HOME}` |
| `$(cmd)` | Sustitución de comando | Ejecuta cmd, devuelve su output | `echo $(date)` |
| `$((expr))` | Expansión aritmética | Calcula matemáticas | `echo $((2+2))` |
| `${VAR:-val}` | Default | Valor si VAR vacía | `${nombre:-Anónimo}` |
| `${#VAR}` | Longitud | Cuenta caracteres | `${#nombre}` |
{% endraw %}

---

## 1. Sustitución de Comandos: `$(comando)`

Ejecuta un comando y **usa su salida como texto**:

```bash
# Sin sustitución
echo date
# date (literal)

# Con sustitución
echo $(date)
# Mon Jan 27 10:30:00 CST 2026 (ejecuta date)
```

### Usos prácticos

```bash
# Guardar resultado en variable
fecha_actual=$(date)
usuario_actual=$(whoami)
directorio=$(pwd)

echo "Usuario: $usuario_actual"
echo "En: $directorio"
echo "Fecha: $fecha_actual"
```

```bash
# Usar directamente en texto
echo "Hoy es $(date +%A)"
echo "Hay $(ls | wc -l) archivos aquí"
echo "Tu IP es $(hostname -I | cut -d' ' -f1)"
```

```bash
# Crear nombres de archivo dinámicos
archivo="backup_$(date +%Y%m%d).tar.gz"
echo $archivo
# backup_20260127.tar.gz

# Crear directorios con fecha
mkdir "logs_$(date +%Y%m%d)"
```

:::exercise{title="Practicar sustitución de comandos" difficulty="2"}

```bash
# 1. Guarda comandos en variables
mi_usuario=$(whoami)
mi_shell=$(echo $SHELL | cut -d'/' -f3)
num_archivos=$(ls ~ | wc -l)

# 2. Usa las variables
echo "Soy $mi_usuario, uso $mi_shell"
echo "Tengo $num_archivos archivos en mi home"

# 3. Crea un nombre de archivo único
archivo_log="log_${mi_usuario}_$(date +%H%M%S).txt"
echo "Archivo: $archivo_log"
```

:::

### Backticks (forma antigua)

También puedes usar backticks, pero `$()` es preferido:

```bash
# Forma moderna (preferida)
echo $(date)

# Forma antigua (funciona pero evítala)
echo `date`
```

**¿Por qué preferir `$()`?**
- Se puede anidar: `$(cmd1 $(cmd2))`
- Más fácil de leer
- Menos errores con comillas

---

## 2. Expansión Aritmética: `$((expresión))`

Bash puede hacer matemáticas:

```bash
echo $((5 + 3))    # 8
echo $((10 - 4))   # 6
echo $((3 * 7))    # 21
echo $((20 / 4))   # 5
echo $((17 % 5))   # 2 (residuo/módulo)
echo $((2 ** 8))   # 256 (potencia)
```

### Con variables

```bash
a=10
b=3

echo $((a + b))    # 13
echo $((a * b))    # 30
echo $((a / b))    # 3 (división entera)
echo $((a % b))    # 1

# Incrementar
a=$((a + 1))
echo $a  # 11
```

### Operadores disponibles

| Operador | Descripción | Ejemplo |
|----------|-------------|---------|
| `+` | Suma | `$((5+3))` → 8 |
| `-` | Resta | `$((5-3))` → 2 |
| `*` | Multiplicación | `$((5*3))` → 15 |
| `/` | División (entera) | `$((5/3))` → 1 |
| `%` | Módulo (residuo) | `$((5%3))` → 2 |
| `**` | Potencia | `$((2**3))` → 8 |

:::exercise{title="Calculadora con $(())" difficulty="2"}

```bash
# Datos
precio=100
cantidad=5
impuesto=16

# Cálculos
subtotal=$((precio * cantidad))
iva=$((subtotal * impuesto / 100))
total=$((subtotal + iva))

# Mostrar
echo "Subtotal: \$$subtotal"
echo "IVA ($impuesto%): \$$iva"
echo "Total: \$$total"
```

:::

---

## 3. Expansión de Variables Avanzada: `${VAR...}`

Además de leer variables, `${}` permite modificarlas al vuelo.

### `${VAR:-default}` - Valor por defecto

Si la variable está **vacía o no existe**, usa el valor default:

```bash
# Variable existe
nombre="Juan"
echo ${nombre:-Anónimo}
# Juan

# Variable vacía
nombre=""
echo ${nombre:-Anónimo}
# Anónimo

# Variable no existe
unset nombre
echo ${nombre:-Anónimo}
# Anónimo
```

**Uso práctico:**

```bash
# Usar variable de entorno o valor por defecto
editor=${EDITOR:-nano}
echo "Usando editor: $editor"

# Puerto con default
puerto=${PUERTO:-8080}
echo "Servidor en puerto $puerto"
```

### `${VAR:=default}` - Asignar si vacía

Similar al anterior, pero **también asigna** el valor:

```bash
unset mi_var
echo ${mi_var:=valor_default}
# valor_default

echo $mi_var
# valor_default (ahora está asignada)
```

{% raw %}
### `${#VAR}` - Longitud de la variable

```bash
nombre="Francisco"
echo ${#nombre}
# 9

password="secreto123"
echo "Tu contraseña tiene ${#password} caracteres"
# Tu contraseña tiene 10 caracteres
```
{% endraw %}

### `${VAR:inicio:longitud}` - Subcadena

```bash
texto="Hola Mundo"

echo ${texto:0:4}    # Hola (desde 0, 4 caracteres)
echo ${texto:5:5}    # Mundo (desde 5, 5 caracteres)
echo ${texto:5}      # Mundo (desde 5 hasta el final)
```

### `${VAR/patron/reemplazo}` - Reemplazar

```bash
archivo="documento.txt"

# Reemplazar primera ocurrencia
echo ${archivo/.txt/.pdf}
# documento.pdf

# Reemplazar todas las ocurrencias (doble /)
texto="uno dos uno tres uno"
echo ${texto//uno/UNO}
# UNO dos UNO tres UNO
```

:::exercise{title="Expansión avanzada" difficulty="3"}

{% raw %}
```bash
# 1. Valores por defecto
nombre=${NOMBRE:-Usuario}
echo "Hola, $nombre"

# 2. Longitud
email="usuario@ejemplo.com"
echo "Tu email tiene ${#email} caracteres"

# 3. Subcadenas
fecha="2026-01-27"
año=${fecha:0:4}
mes=${fecha:5:2}
dia=${fecha:8:2}
echo "Año: $año, Mes: $mes, Día: $dia"

# 4. Reemplazo
archivo="foto_vacaciones.jpg"
echo "Original: $archivo"
echo "Thumbnail: ${archivo/.jpg/_thumb.jpg}"
```
{% endraw %}

:::

---

## 4. Expansión de Llaves: `{...}`

Genera múltiples strings a partir de un patrón:

### Lista de valores

```bash
echo {a,b,c}
# a b c

echo archivo_{uno,dos,tres}.txt
# archivo_uno.txt archivo_dos.txt archivo_tres.txt
```

### Secuencias

```bash
echo {1..5}
# 1 2 3 4 5

echo {a..e}
# a b c d e

echo {01..10}
# 01 02 03 04 05 06 07 08 09 10

echo archivo{1..3}.txt
# archivo1.txt archivo2.txt archivo3.txt
```

### Usos prácticos

```bash
# Crear múltiples directorios
mkdir proyecto/{src,tests,docs,data}

# Crear archivos numerados
touch log_{01..05}.txt

# Backup con fecha
cp config.txt config.txt.{bak,$(date +%Y%m%d)}
```

:::exercise{title="Expansión de llaves" difficulty="2"}

```bash
# 1. Lista
echo {perro,gato,pez}

# 2. Secuencia numérica
echo {1..10}

# 3. Secuencia con padding
echo {01..10}

# 4. Crear estructura de proyecto
mkdir -p mi_proyecto/{src,tests,docs}
ls mi_proyecto/

# 5. Múltiples extensiones
touch archivo.{txt,md,py}
ls archivo.*
```

:::

---

## Combinando Todo

El verdadero poder viene de combinar tipos de expansión:

```bash
# Variables + sustitución de comando
usuario=$(whoami)
fecha=$(date +%Y%m%d)
backup_dir="/backups/${usuario}/${fecha}"
echo $backup_dir
# /backups/tu_usuario/20260127

# Aritmética + variables
archivos=$(ls | wc -l)
echo "Tienes $archivos archivos, el doble sería $((archivos * 2))"

# Default + sustitución
LOG_DIR=${LOG_DIR:-/var/log}
ultimo_log=$(ls -t $LOG_DIR/*.log 2>/dev/null | head -1)
echo "Último log: ${ultimo_log:-No hay logs}"

# Todo junto
proyecto=${1:-mi_proyecto}
mkdir -p "$proyecto"/{src,tests,docs}
echo "# $proyecto" > "$proyecto/README.md"
echo "Creado el $(date)" >> "$proyecto/README.md"
echo "Proyecto $proyecto creado"
```

:::exercise{title="Proyecto completo con expansiones" difficulty="3"}

```bash
# Crear estructura de proyecto con fecha
nombre_proyecto="app"
fecha=$(date +%Y%m%d_%H%M%S)
dir_proyecto="${nombre_proyecto}_${fecha}"

# Crear estructura
mkdir -p "$dir_proyecto"/{src,tests,docs,data/{raw,processed}}

# Crear README
cat << EOF > "$dir_proyecto/README.md"
# $nombre_proyecto

Creado: $(date)
Usuario: $(whoami)
Directorio: $(pwd)/$dir_proyecto

## Estructura
- src/ - código fuente
- tests/ - pruebas
- docs/ - documentación
- data/ - datos
EOF

# Verificar
echo "Proyecto creado:"
ls -R "$dir_proyecto"
echo ""
echo "README:"
cat "$dir_proyecto/README.md"
```

:::

---

## Errores Comunes

### Confundir `$()` con `$(())`

```bash
# $() ejecuta un comando
echo $(pwd)          # /home/usuario

# $(()) hace matemáticas
echo $((2 + 2))      # 4

# ERROR común
echo $((pwd))        # Error - pwd no es una expresión matemática
echo $(2 + 2)        # Error - "2" no es un comando
```

### Olvidar comillas con espacios

```bash
archivo="mi archivo.txt"

# MAL - se interpreta como dos argumentos
ls $archivo
# ls: cannot access 'mi': No such file

# BIEN - comillas preservan el espacio
ls "$archivo"
```

---

## Tabla Resumen

{% raw %}
| Sintaxis | Nombre | Ejemplo | Resultado |
|----------|--------|---------|-----------|
| `$VAR` | Variable | `echo $HOME` | `/home/user` |
| `${VAR}` | Variable segura | `echo ${USER}` | `user` |
| `$(cmd)` | Sustitución comando | `$(date +%Y)` | `2026` |
| `$((expr))` | Aritmética | `$((5*3))` | `15` |
| `${VAR:-def}` | Default | `${X:-none}` | `none` |
| `${#VAR}` | Longitud | `${#HOME}` | `10` |
| `${VAR:0:4}` | Subcadena | `${HOME:0:4}` | `/hom` |
| `{a,b,c}` | Lista | `echo {1,2,3}` | `1 2 3` |
| `{1..5}` | Secuencia | `echo {1..5}` | `1 2 3 4 5` |
{% endraw %}

---

> **Siguiente:** Ahora sí - vamos a poner todo junto en **scripts de Bash**.
