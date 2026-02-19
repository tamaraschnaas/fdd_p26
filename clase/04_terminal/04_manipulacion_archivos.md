# Manipulación de Archivos

![Manipulación de archivos - Fern organizando objetos mágicos en cajas](./images/04_files_fern_organizing.png)

Crear, copiar, mover y eliminar archivos y directorios.

> **Atajos para practicar:** Usa `Tab` para autocompletar nombres de archivos y `Ctrl + R` para buscar comandos que ya ejecutaste.

---

## Crear Directorios: `mkdir`

**M**a**k**e **Dir**ectory

```bash
# Crear un directorio
mkdir mi_carpeta

# Crear varios directorios
mkdir dir1 dir2 dir3

# Crear directorios anidados (-p = parents)
mkdir -p proyecto/src/utils

# Sin -p esto falla si 'proyecto' no existe
mkdir proyecto/src/utils  # Error!
```

:::exercise{title="Crear estructura de proyecto" difficulty="1"}

Crea esta estructura con un solo comando:

```
mi_proyecto/
├── src/
├── tests/
└── docs/
```

**Solución:** `mkdir -p mi_proyecto/{src,tests,docs}`

:::

---

## Crear Archivos: `touch`

`touch` crea archivos vacíos (o actualiza la fecha de modificación si ya existen).

```bash
# Crear un archivo vacío
touch archivo.txt

# Crear varios archivos
touch file1.txt file2.txt file3.txt

# Crear archivos con patrón
touch test_{01..05}.py
# Crea: test_01.py, test_02.py, ..., test_05.py
```

> **Atajo:** Usa llaves `{}` para crear múltiples archivos de una vez.

---

## Ver Contenido de Archivos

### `cat` - Concatenar/mostrar

```bash
# Ver contenido de un archivo
cat archivo.txt

# Ver varios archivos
cat file1.txt file2.txt

# Numerar líneas
cat -n archivo.txt
```

### `less` - Ver archivos largos

```bash
less archivo_largo.txt
```

Dentro de `less`:
- `q` - salir
- `↑/↓` o `j/k` - navegar
- `Space` - página siguiente
- `b` - página anterior
- `/texto` - buscar
- `n` - siguiente resultado

### `head` y `tail` - Inicio y final

```bash
# Primeras 10 líneas
head archivo.txt

# Primeras 5 líneas
head -n 5 archivo.txt

# Últimas 10 líneas
tail archivo.txt

# Últimas 20 líneas
tail -n 20 archivo.txt

# Ver archivo en tiempo real (útil para logs)
tail -f /var/log/syslog
```

---

![Copiar archivos - Rei Ayanami clones (copias del original)](./images/04_cp_rei_clones.png)

## Copiar: `cp`

**C**o**p**y

```bash
# Copiar archivo
cp original.txt copia.txt

# Copiar a otro directorio
cp archivo.txt ~/Documents/

# Copiar con nuevo nombre en destino
cp archivo.txt ~/Documents/nuevo_nombre.txt

# Copiar directorio completo (-r = recursive)
cp -r carpeta/ carpeta_backup/

# Copiar mostrando progreso (-v = verbose)
cp -rv carpeta/ destino/
```

### Opciones útiles

| Opción | Significado |
|--------|-------------|
| `-r` | Recursivo (necesario para directorios) |
| `-v` | Verbose (muestra qué está copiando) |
| `-i` | Interactivo (pregunta antes de sobrescribir) |
| `-n` | No sobrescribir archivos existentes |

---

## Mover y Renombrar: `mv`

**M**o**v**e - sirve para mover **y** renombrar.

```bash
# Renombrar archivo
mv nombre_viejo.txt nombre_nuevo.txt

# Mover a otro directorio
mv archivo.txt ~/Documents/

# Mover y renombrar
mv archivo.txt ~/Documents/nuevo_nombre.txt

# Mover múltiples archivos a un directorio
mv file1.txt file2.txt file3.txt ~/destino/

# Mover directorio
mv carpeta/ nuevo_lugar/
```

> **Nota:** `mv` no necesita `-r` para directorios, a diferencia de `cp`.

### Opciones útiles

| Opción | Significado |
|--------|-------------|
| `-i` | Pregunta antes de sobrescribir |
| `-n` | No sobrescribir existentes |
| `-v` | Muestra qué está moviendo |

---

![Eliminar archivos - EVA-01 en modo berserk destruyendo datos](./images/04_rm_eva_berserk.png)

## Eliminar: `rm`

**R**e**m**ove

> **⚠️ CUIDADO:** `rm` es permanente. No hay papelera de reciclaje.

```bash
# Eliminar archivo
rm archivo.txt

# Eliminar con confirmación (-i = interactive)
rm -i archivo.txt

# Eliminar múltiples archivos
rm file1.txt file2.txt file3.txt

# Eliminar directorio vacío
rmdir carpeta_vacia/

# Eliminar directorio con contenido (-r = recursive)
rm -r carpeta/

# Forzar eliminación sin preguntas (-f = force)
rm -rf carpeta/
```

### El peligroso `rm -rf`

```bash
# ⚠️ NUNCA hagas esto:
rm -rf /                    # Borra TODO el sistema
rm -rf ~                    # Borra tu home completo
rm -rf *                    # Borra todo en el directorio actual

# Siempre verifica antes con ls:
ls carpeta_a_borrar/
rm -rf carpeta_a_borrar/
```

### Tip: Usa `-i` para estar seguro

```bash
# Agrega un alias de seguridad
alias rm='rm -i'
```

---

## Eliminar Directorios: `rmdir`

Solo elimina directorios **vacíos**.

```bash
# Eliminar directorio vacío
rmdir carpeta_vacia/

# Si tiene contenido:
rmdir carpeta_con_archivos/
# Error: Directory not empty

# Para eso usa rm -r
rm -r carpeta_con_archivos/
```

---

## Ejercicios Prácticos

:::exercise{title="Organizar archivos" difficulty="2"}

1. Crea esta estructura:
   ```bash
   mkdir -p ejercicio/{entrada,salida,respaldo}
   ```

2. Crea archivos de prueba:
   ```bash
   touch ejercicio/entrada/data{1..3}.txt
   ```

3. Copia los archivos a `respaldo`:
   ```bash
   cp ejercicio/entrada/*.txt ejercicio/respaldo/
   ```

4. Mueve un archivo a `salida`:
   ```bash
   mv ejercicio/entrada/data1.txt ejercicio/salida/
   ```

5. Verifica la estructura:
   ```bash
   ls -R ejercicio/
   ```

> **Practica el atajo:** Usa `Tab` para autocompletar las rutas.

:::

:::exercise{title="Renombrar en masa" difficulty="2"}

1. Crea archivos de prueba:
   ```bash
   touch file_{a,b,c}.txt
   ```

2. Renómbralos manualmente uno por uno:
   ```bash
   mv file_a.txt archivo_a.txt
   ```

3. Verifica con `ls`

> **Atajo:** Después de escribir `mv file_a.txt`, presiona `!$` para insertar el último argumento y solo cambia el nombre.

:::

---

## Resumen de Comandos

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `mkdir` | Crear directorio | `mkdir proyecto` |
| `mkdir -p` | Crear directorios anidados | `mkdir -p a/b/c` |
| `touch` | Crear archivo vacío | `touch file.txt` |
| `cat` | Ver contenido | `cat file.txt` |
| `less` | Ver archivos largos | `less log.txt` |
| `head` | Ver inicio | `head -n 5 file.txt` |
| `tail` | Ver final | `tail -n 10 file.txt` |
| `tail -f` | Ver en tiempo real | `tail -f log.txt` |
| `cp` | Copiar | `cp a.txt b.txt` |
| `cp -r` | Copiar directorio | `cp -r dir1/ dir2/` |
| `mv` | Mover/renombrar | `mv old.txt new.txt` |
| `rm` | Eliminar archivo | `rm file.txt` |
| `rm -r` | Eliminar directorio | `rm -r carpeta/` |
| `rmdir` | Eliminar dir vacío | `rmdir vacio/` |

---

---

## Más Ejercicios

:::exercise{title="Estructura de proyecto Python" difficulty="2"}

Crea la estructura típica de un proyecto Python:

```bash
mkdir -p mi_proyecto/{src,tests,docs,data/{raw,processed}}
touch mi_proyecto/README.md
touch mi_proyecto/requirements.txt
touch mi_proyecto/src/__init__.py
touch mi_proyecto/src/main.py
touch mi_proyecto/tests/__init__.py
touch mi_proyecto/tests/test_main.py
```

Verifica con: `ls -R mi_proyecto/`

:::

:::exercise{title="Backup y restauración" difficulty="2"}

Practica hacer backups:

```bash
# Crea archivos de prueba
mkdir original
echo "datos importantes" > original/datos.txt
echo "config" > original/config.txt

# Haz backup con fecha
cp -r original/ backup_$(date +%Y%m%d)/

# Verifica
ls -la
cat backup_*/datos.txt
```

:::

:::exercise{title="Renombrado masivo simulado" difficulty="3"}

1. Crea archivos con nombres desordenados:
   ```bash
   mkdir renombrar && cd renombrar
   touch "archivo con espacios.txt"
   touch "MAYUSCULAS.txt"
   touch "foto_2024_01_15.jpg"
   ```

2. Renómbralos uno por uno:
   ```bash
   mv "archivo con espacios.txt" archivo_con_guiones.txt
   mv "MAYUSCULAS.txt" minusculas.txt
   ```

3. Verifica: `ls`

**Nota:** Para renombrado masivo avanzado se usa `rename` o scripts.

:::

:::exercise{title="Limpieza segura" difficulty="2"}

Practica eliminar de forma segura:

```bash
# Crea archivos para borrar
mkdir basura
touch basura/temp{1..5}.txt

# SIEMPRE verifica antes de borrar
ls basura/

# Usa -i para confirmar cada archivo
rm -i basura/*.txt

# O borra todo el directorio después de verificar
rm -r basura/
```

:::

---

## Prompts para LLM

:::prompt{title="Organizar descargas" for="ChatGPT/Claude"}

Tengo mi carpeta de Descargas llena de archivos desordenados. Dame comandos de terminal para:

1. Listar todos los archivos por tipo (pdf, jpg, zip, etc.)
2. Crear carpetas para cada tipo
3. Mover los archivos a sus carpetas correspondientes

Estoy en [Linux/macOS/WSL2].

:::

:::prompt{title="Recuperar archivo borrado" for="ChatGPT/Claude"}

Accidentalmente borré un archivo importante con `rm`. ¿Hay alguna forma de recuperarlo en Linux?

También explícame cómo puedo prevenir esto en el futuro (alias, papelera de terminal, etc.)

:::

:::prompt{title="Copiar entre WSL2 y Windows" for="ChatGPT/Claude"}

Necesito copiar archivos entre mi sistema WSL2 y Windows frecuentemente. 

1. ¿Cuál es la mejor forma de hacerlo?
2. ¿Dónde están mis archivos de Windows desde WSL2?
3. ¿Dónde están mis archivos de WSL2 desde Windows?
4. ¿Hay consideraciones de permisos o rendimiento?

:::

---

## Recordatorio de Atajos

Mientras practicas esta sección:

| Atajo | Úsalo para |
|-------|------------|
| `Tab` | Autocompletar nombres de archivos |
| `Ctrl + R` | Buscar comandos que ya usaste |
| `↑` | Repetir comandos anteriores |
| `!$` | Reusar el último argumento |
| `Ctrl + W` | Borrar si te equivocas |
