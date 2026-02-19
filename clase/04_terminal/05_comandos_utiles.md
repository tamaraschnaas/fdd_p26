# Comandos Útiles

![Comandos útiles - Frieren con su colección de hechizos prácticos](./images/05_commands_frieren_spells.png)

:::exercise{title="Comandos que usarás en Bandit" difficulty="2"}

Estos comandos aparecen en los niveles de Bandit. Practícalos:

```bash
# Ver archivos ocultos
ls -la

# Leer contenido de archivos
cat archivo.txt

# Buscar texto en archivos
grep "palabra" archivo.txt

# Buscar archivos
find . -name "nombre"

# Ver tipo de archivo
file archivo
```

**Tip:** En Bandit, muchas contraseñas están en archivos con nombres raros o están ocultos. ¡Usa estos comandos!

:::

Herramientas poderosas para buscar, filtrar y procesar información.

> **Practica:** Sigue usando `Tab` para autocompletar y `Ctrl + R` para buscar comandos anteriores.

---

![Find - Lain buscando en la Wired entre datos infinitos](./images/05_find_lain_wired.png)

## Búsqueda de Archivos: `find`

Busca archivos y directorios por nombre, tipo, fecha, tamaño, etc.

```bash
# Buscar por nombre
find . -name "archivo.txt"

# Buscar ignorando mayúsculas/minúsculas
find . -iname "archivo.txt"

# Buscar con comodines
find . -name "*.py"

# Buscar solo directorios
find . -type d -name "tests"

# Buscar solo archivos
find . -type f -name "*.txt"

# Buscar en una ruta específica
find /home/usuario -name "config*"
```

### Opciones útiles de `find`

| Opción | Significado | Ejemplo |
|--------|-------------|---------|
| `-name` | Por nombre exacto | `-name "file.txt"` |
| `-iname` | Nombre (ignora mayúsculas) | `-iname "File.txt"` |
| `-type f` | Solo archivos | `-type f` |
| `-type d` | Solo directorios | `-type d` |
| `-mtime -7` | Modificados últimos 7 días | `-mtime -7` |
| `-size +1M` | Mayores a 1 MB | `-size +1M` |

### Ejemplos prácticos

```bash
# Archivos Python modificados hoy
find . -name "*.py" -mtime 0

# Archivos grandes (más de 100MB)
find . -type f -size +100M

# Eliminar archivos .tmp (cuidado!)
find . -name "*.tmp" -delete
```

---

![Grep - MAGI system de Evangelion analizando patrones](./images/05_grep_magi_analysis.png)

## Búsqueda en Contenido: `grep`

**G**lobal **R**egular **E**xpression **P**rint - busca texto dentro de archivos.

```bash
# Buscar palabra en un archivo
grep "error" log.txt

# Buscar ignorando mayúsculas
grep -i "error" log.txt

# Buscar en múltiples archivos
grep "import" *.py

# Buscar recursivamente en directorios
grep -r "TODO" .

# Mostrar número de línea
grep -n "error" log.txt

# Mostrar contexto (líneas antes/después)
grep -C 3 "error" log.txt

# Contar ocurrencias
grep -c "error" log.txt
```

### Opciones útiles de `grep`

| Opción | Significado |
|--------|-------------|
| `-i` | Ignora mayúsculas/minúsculas |
| `-r` | Recursivo (busca en subdirectorios) |
| `-n` | Muestra número de línea |
| `-c` | Cuenta ocurrencias |
| `-l` | Solo muestra nombres de archivos |
| `-v` | Invierte (líneas que NO coinciden) |
| `-A n` | Muestra n líneas después |
| `-B n` | Muestra n líneas antes |
| `-C n` | Muestra n líneas antes y después |

### Ejemplos prácticos

```bash
# Buscar funciones en código Python
grep -n "def " *.py

# Buscar TODOs en todo el proyecto
grep -rn "TODO" --include="*.py" .

# Ver errores en logs, ignorando líneas con "debug"
grep "error" log.txt | grep -v "debug"
```

---

![Pipes - Flujo de LCL en Evangelion conectando sistemas](./images/05_pipes_lcl_flow.png)

## Redirección y Pipes

### Redirección de salida

```bash
# Guardar salida en archivo (sobrescribe)
ls -la > listado.txt

# Agregar al final del archivo
echo "nueva línea" >> archivo.txt

# Redirigir errores
comando 2> errores.txt

# Redirigir salida y errores
comando > salida.txt 2>&1
# O en bash moderno:
comando &> todo.txt
```

### Pipes (`|`)

El pipe envía la salida de un comando como entrada del siguiente.

```bash
# Filtrar salida
ls -la | grep ".txt"

# Contar archivos
ls | wc -l

# Ver últimos procesos
ps aux | tail -10

# Buscar en historial
history | grep "git"

# Ordenar y eliminar duplicados
cat lista.txt | sort | uniq
```

### Ejemplos prácticos

```bash
# Los 5 archivos más grandes
ls -lS | head -5

# Contar líneas de código Python
find . -name "*.py" | xargs wc -l

# Procesos que usan más memoria
ps aux | sort -k 4 -r | head -10
```

---

## Información del Sistema

### Espacio en disco: `df`

```bash
# Ver espacio en disco
df -h

# Solo sistema de archivos local
df -h --local
```

### Tamaño de directorios: `du`

```bash
# Tamaño del directorio actual
du -sh .

# Tamaño de cada subdirectorio
du -h --max-depth=1

# Ordenado por tamaño
du -h --max-depth=1 | sort -h
```

### Información del sistema: `uname`

```bash
# Sistema operativo
uname -s

# Toda la información
uname -a

# Distribución de Linux
cat /etc/os-release
```

---

## Procesos

### Ver procesos: `ps`

```bash
# Tus procesos
ps

# Todos los procesos
ps aux

# Buscar un proceso específico
ps aux | grep python
```

### Monitor interactivo: `top` / `htop`

```bash
# Monitor básico
top

# Monitor mejorado (si está instalado)
htop
```

Dentro de `top`/`htop`:
- `q` - salir
- `k` - matar proceso
- `M` - ordenar por memoria
- `P` - ordenar por CPU

### Matar procesos: `kill`

```bash
# Matar por PID
kill 1234

# Forzar terminación
kill -9 1234

# Matar por nombre
killall python
pkill firefox
```

---

## Permisos de Archivos

### Entender permisos

```
-rwxr-xr-- 1 usuario grupo 4096 Jan 20 10:00 script.sh
```

| Parte | Significado |
|-------|-------------|
| `-` | Tipo (- archivo, d directorio) |
| `rwx` | Permisos del dueño (read, write, execute) |
| `r-x` | Permisos del grupo |
| `r--` | Permisos de otros |

### Cambiar permisos: `chmod`

```bash
# Dar permiso de ejecución al dueño
chmod u+x script.sh

# Dar permiso de lectura a todos
chmod a+r archivo.txt

# Quitar permiso de escritura a otros
chmod o-w archivo.txt

# Usando números (rwx = 4+2+1 = 7)
chmod 755 script.sh    # rwxr-xr-x
chmod 644 archivo.txt  # rw-r--r--
```

### Cambiar dueño: `chown`

```bash
# Cambiar dueño
sudo chown usuario archivo.txt

# Cambiar dueño y grupo
sudo chown usuario:grupo archivo.txt

# Recursivo
sudo chown -R usuario:grupo carpeta/
```

---

## Descarga de Archivos

### `wget`

```bash
# Descargar archivo
wget https://example.com/archivo.zip

# Guardar con otro nombre
wget -O nuevo_nombre.zip https://example.com/archivo.zip

# Descargar en silencio
wget -q https://example.com/archivo.zip
```

### `curl`

```bash
# Ver contenido de una URL
curl https://api.example.com/data

# Guardar en archivo
curl -o archivo.zip https://example.com/archivo.zip

# Seguir redirecciones
curl -L https://example.com/redirect
```

---

## Ejercicios

:::exercise{title="Buscar y filtrar" difficulty="2"}

1. Encuentra todos los archivos `.md` en el directorio actual:
   ```bash
   find . -name "*.md"
   ```

2. Busca la palabra "error" en todos los archivos de texto:
   ```bash
   grep -r "error" --include="*.txt" .
   ```

3. Cuenta cuántas líneas tienen tus archivos Python:
   ```bash
   find . -name "*.py" | xargs wc -l
   ```

:::

:::exercise{title="Pipes y redirección" difficulty="2"}

1. Lista todos los archivos y guárdalos en `listado.txt`:
   ```bash
   ls -la > listado.txt
   ```

2. Encuentra los 3 directorios más grandes:
   ```bash
   du -h --max-depth=1 | sort -hr | head -4
   ```

3. Cuenta cuántos procesos están corriendo:
   ```bash
   ps aux | wc -l
   ```

:::

---

## Resumen de Comandos

| Comando | Descripción |
|---------|-------------|
| `find . -name "*.py"` | Buscar archivos |
| `grep "texto" archivo` | Buscar en contenido |
| `grep -r "texto" .` | Buscar recursivamente |
| `comando > archivo` | Redirigir salida |
| `comando >> archivo` | Agregar a archivo |
| `cmd1 \| cmd2` | Pipe |
| `df -h` | Espacio en disco |
| `du -sh` | Tamaño de directorio |
| `ps aux` | Ver procesos |
| `kill PID` | Matar proceso |
| `chmod +x script.sh` | Dar permisos de ejecución |
| `wget URL` | Descargar archivo |

---

---

## Más Ejercicios

:::exercise{title="Detective de archivos" difficulty="2"}

Encuentra información en tu sistema:

```bash
# ¿Cuántos archivos .py hay en tu home?
find ~ -name "*.py" 2>/dev/null | wc -l

# ¿Cuáles son los 5 archivos más grandes?
find ~ -type f -exec ls -s {} \; 2>/dev/null | sort -n -r | head -5

# ¿Qué archivos modificaste hoy?
find ~ -type f -mtime 0 2>/dev/null | head -20
```

:::

:::exercise{title="Grep avanzado" difficulty="3"}

Crea un archivo de log para practicar:

```bash
cat << 'EOF' > practice.log
2026-01-20 10:30:15 INFO User login successful
2026-01-20 10:31:22 ERROR Database connection failed
2026-01-20 10:32:45 INFO User logout
2026-01-20 10:33:10 WARNING Disk space low
2026-01-20 10:34:55 ERROR File not found: config.yml
2026-01-20 10:35:30 INFO Backup completed
EOF
```

Ahora practica:

```bash
# Solo errores
grep "ERROR" practice.log

# Errores y warnings
grep -E "ERROR|WARNING" practice.log

# Todo menos INFO
grep -v "INFO" practice.log

# Con número de línea
grep -n "ERROR" practice.log

# Contar cada tipo
grep -c "ERROR" practice.log
grep -c "INFO" practice.log
```

:::

:::exercise{title="Pipeline maestro" difficulty="3"}

Crea un archivo con datos:

```bash
cat << 'EOF' > datos.csv
nombre,edad,ciudad
Ana,25,CDMX
Carlos,30,Monterrey
Diana,25,Guadalajara
Eduardo,35,CDMX
Fernanda,28,Monterrey
EOF
```

Usa pipes para:

```bash
# Ver sin encabezado
tail -n +2 datos.csv

# Personas de CDMX
grep "CDMX" datos.csv

# Contar personas por ciudad
cut -d',' -f3 datos.csv | tail -n +2 | sort | uniq -c

# Ordenar por edad
tail -n +2 datos.csv | sort -t',' -k2 -n
```

:::

:::exercise{title="Monitor de sistema" difficulty="2"}

Practica comandos de monitoreo:

```bash
# Espacio en disco
df -h

# Uso de memoria
free -h

# Top 5 procesos por CPU
ps aux --sort=-%cpu | head -6

# Top 5 procesos por memoria
ps aux --sort=-%mem | head -6

# Tu directorio más pesado
du -h --max-depth=1 ~ 2>/dev/null | sort -hr | head -5
```

:::

---

## Prompts para LLM

:::prompt{title="Buscar texto en proyecto" for="ChatGPT/Claude"}

Tengo un proyecto de código y necesito encontrar todas las ocurrencias de [palabra/función/variable].

El proyecto está en: [ruta]
Lenguajes: [Python/JavaScript/etc]

Dame el comando grep más eficiente para:
1. Buscar la palabra exacta
2. Ver contexto (líneas antes y después)
3. Ignorar ciertos directorios (node_modules, __pycache__, etc.)

:::

:::prompt{title="Analizar logs" for="ChatGPT/Claude"}

Tengo un archivo de logs grande y necesito:
1. Encontrar todos los errores de las últimas 24 horas
2. Contar cuántas veces aparece cada tipo de error
3. Extraer solo las líneas con un patrón específico

El formato de mis logs es:
```
[pega ejemplo de línea de log]
```

Dame los comandos con grep, awk, sed o lo que sea más eficiente.

:::

:::prompt{title="Comando complejo" for="ChatGPT/Claude"}

Necesito hacer esto en la terminal:
[describe lo que quieres hacer]

Ejemplo de mi estructura de archivos:
```
[muestra tu estructura]
```

Dame el comando o pipeline de comandos para lograrlo. Explícame cada parte.

:::

:::prompt{title="Permisos de archivo" for="ChatGPT/Claude"}

Tengo este error de permisos:

```
[pega el error]
```

El archivo tiene estos permisos (resultado de ls -la):
```
[pega los permisos]
```

¿Qué permisos necesito y cuál es el comando chmod correcto? Explícame qué significa cada número/letra.

:::

---

## Recordatorio de Atajos

| Atajo | Úsalo para |
|-------|------------|
| `Tab` | Autocompletar |
| `Ctrl + R` | Buscar comandos previos con grep, find, etc. |
| `Ctrl + C` | Cancelar comando largo (como find en todo /) |
| `Ctrl + L` | Limpiar pantalla después de mucha salida |
