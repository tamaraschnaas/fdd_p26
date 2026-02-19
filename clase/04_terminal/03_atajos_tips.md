# Atajos y Productividad

![Atajos - Lain tecleando a alta velocidad con efectos de glitch](./images/03_shortcuts_lain_typing.png)

:::exercise{title="Atajos esenciales para Bandit" difficulty="1"}

Practica estos atajos que te ayudarán en Bandit:

1. `Tab` - autocompletar nombres de archivos (muy útil cuando no sabes qué hay)
2. `Ctrl + C` - cancelar un comando (si algo se queda pegado)
3. `↑` - repetir comandos anteriores
4. `Ctrl + L` - limpiar pantalla

**Usa estos atajos mientras resuelves los niveles de Bandit.**

:::

Dominar estos atajos te hará **mucho** más eficiente. Apréndelos ahora y practícalos en las siguientes secciones.

> **Importante:** Practica estos atajos mientras avanzas en el módulo. Cada vez que escribas un comando largo, piensa si hay un atajo que puedas usar.

---

## Atajos de Teclado Esenciales

### Navegación en la Línea

| Atajo | Acción |
|-------|--------|
| `Ctrl + A` | Ir al **inicio** de la línea |
| `Ctrl + E` | Ir al **final** de la línea |
| `Ctrl + ←` o `Alt + B` | Retroceder una palabra |
| `Ctrl + →` o `Alt + F` | Avanzar una palabra |

:::exercise{title="Practica navegación" difficulty="1"}

1. Escribe (sin ejecutar): `echo "esta es una linea muy larga de ejemplo"`
2. Presiona `Ctrl + A` - cursor al inicio
3. Presiona `Ctrl + E` - cursor al final
4. Presiona `Ctrl + ←` varias veces - salta palabras
5. Presiona `Enter` para ejecutar

:::

### Edición de Texto

| Atajo | Acción |
|-------|--------|
| `Ctrl + U` | Borrar desde cursor hasta el **inicio** |
| `Ctrl + K` | Borrar desde cursor hasta el **final** |
| `Ctrl + W` | Borrar la palabra anterior |
| `Alt + D` | Borrar la palabra siguiente |
| `Ctrl + Y` | Pegar lo último borrado (yank) |
| `Ctrl + _` | Deshacer |

:::exercise{title="Practica edición" difficulty="1"}

1. Escribe: `echo "texto que voy a borrar parcialmente"`
2. Presiona `Ctrl + A` para ir al inicio
3. Presiona `Ctrl + K` - borra todo
4. Presiona `Ctrl + Y` - recupera el texto
5. Presiona `Ctrl + W` - borra última palabra

:::

### Control de Procesos

| Atajo | Acción |
|-------|--------|
| `Ctrl + C` | **Cancelar** comando actual |
| `Ctrl + Z` | **Suspender** comando (background) |
| `Ctrl + D` | Salir / EOF (cierra terminal si está vacía) |
| `Ctrl + L` | **Limpiar** pantalla (como `clear`) |

### Búsqueda en Historial

| Atajo | Acción |
|-------|--------|
| `↑` / `↓` | Navegar historial |
| `Ctrl + R` | **Buscar** en historial (reverse search) |
| `Ctrl + G` | Salir de búsqueda |
| `!!` | Repetir último comando |
| `!$` | Último argumento del comando anterior |

---

![Tab completion - Frieren usando magia de autocompletado](./images/03_tab_frieren_autocomplete.png)

## Tab Completion (Autocompletado)

El **Tab** es tu mejor amigo. Presiona `Tab` para:

### Autocompletar nombres de archivos

```bash
cd Doc[Tab]
# Se completa a: cd Documents/

ls Down[Tab]
# Se completa a: ls Downloads/
```

### Ver opciones disponibles

```bash
cd D[Tab][Tab]
# Muestra: Desktop/  Documents/  Downloads/
```

### Autocompletar comandos

```bash
sys[Tab][Tab]
# Muestra todos los comandos que empiezan con "sys"
```

:::exercise{title="Practica Tab completion" difficulty="1"}

1. Escribe `cd /u` y presiona `Tab` → se completa a `/usr/`
2. Escribe `ls /e` y presiona `Tab` → se completa a `/etc/`
3. Escribe `cd ~` y presiona `Tab` dos veces → muestra carpetas en tu home

:::

---

![Historial - Frieren recordando hechizos del pasado (memorias de 1000 años)](./images/03_history_frieren_memories.png)

## Historial de Comandos

### Navegar con flechas

```bash
↑    # Comando anterior
↓    # Comando siguiente
```

### Buscar con `Ctrl + R`

```bash
# Presiona Ctrl + R
(reverse-i-search)`': 

# Escribe parte del comando que buscas
(reverse-i-search)`git': git push origin main

# Presiona Enter para ejecutar o → para editar
```

### Ver historial completo

```bash
history
# Muestra todos los comandos numerados

history | tail -20
# Muestra los últimos 20
```

### Ejecutar comandos del historial

```bash
!!          # Ejecuta el último comando
!42         # Ejecuta el comando #42 del historial
!git        # Ejecuta el último comando que empezó con "git"
!$          # Usa el último argumento del comando anterior
```

### Ejemplo práctico de `!$`

```bash
mkdir nuevo_proyecto
cd !$
# Equivale a: cd nuevo_proyecto
```

---

## Expansión y Comodines

### Comodines (Wildcards)

| Comodín | Significado | Ejemplo |
|---------|-------------|---------|
| `*` | Cualquier cosa | `*.txt` = todos los .txt |
| `?` | Un solo carácter | `archivo?.txt` = archivo1.txt, archivoA.txt |
| `[abc]` | Uno de estos caracteres | `archivo[123].txt` |
| `[a-z]` | Rango de caracteres | `[a-z]*.txt` |

```bash
# Listar todos los archivos .py
ls *.py

# Listar archivos que empiezan con "test"
ls test*

# Listar archivo1.txt, archivo2.txt, archivo3.txt
ls archivo[1-3].txt
```

### Llaves (Brace Expansion)

```bash
# Crear múltiples archivos
touch archivo{1,2,3}.txt
# Crea: archivo1.txt, archivo2.txt, archivo3.txt

# Crear secuencia
touch test{01..10}.py
# Crea: test01.py, test02.py, ..., test10.py

# Combinar extensiones
ls *.{py,txt,md}
# Lista archivos .py, .txt y .md
```

---

## El Pipe `|` - Conectar Comandos

El **pipe** (tubo) es uno de los conceptos más poderosos de Unix. Te permite **conectar la salida de un comando con la entrada de otro**.

### ¿Qué es un Pipe?

```
comando1 | comando2
```

Significa: "toma lo que produce `comando1` y pásaselo a `comando2`".

```mermaid
graph LR
    A[comando1] -->|salida| B[pipe |]
    B -->|entrada| C[comando2]
    C --> D[resultado]
```

### Analogía

Piensa en una fábrica con máquinas conectadas por tubos:
- La **primera máquina** produce algo
- El **tubo** lleva ese producto a la siguiente máquina
- La **segunda máquina** lo procesa y produce el resultado final

### Ejemplos Básicos

```bash
# Sin pipe: ls muestra archivos en pantalla
ls -la

# Con pipe: ls pasa su salida a 'head' que muestra solo las primeras 5 líneas
ls -la | head -5

# Sin pipe: cat muestra todo el archivo
cat archivo_largo.txt

# Con pipe: cat pasa el contenido a 'grep' que filtra líneas con "error"
cat archivo.log | grep "error"
```

### Ejemplos Prácticos

```bash
# Ver los últimos 10 comandos del historial
history | tail -10

# Contar cuántos archivos hay en un directorio
ls | wc -l

# Buscar un proceso específico
ps aux | grep python

# Ver archivos ordenados por tamaño (más grande primero)
ls -la | sort -k5 -n -r | head -10

# Buscar "TODO" en archivos y contar ocurrencias
grep -r "TODO" . | wc -l
```

### Encadenando Múltiples Pipes

Puedes conectar muchos comandos:

```bash
# comando1 | comando2 | comando3 | comando4

# Ejemplo: encontrar las 5 palabras más frecuentes en un archivo
cat texto.txt | tr ' ' '\n' | sort | uniq -c | sort -nr | head -5
```

**¿Qué hace cada parte?**

| Paso | Comando | Qué hace |
|------|---------|----------|
| 1 | `cat texto.txt` | Lee el archivo |
| 2 | `tr ' ' '\n'` | Reemplaza espacios por saltos de línea (una palabra por línea) |
| 3 | `sort` | Ordena alfabéticamente |
| 4 | `uniq -c` | Cuenta repeticiones consecutivas |
| 5 | `sort -nr` | Ordena por número (mayor primero) |
| 6 | `head -5` | Muestra solo los primeros 5 |

### Comandos Útiles para Usar con Pipes

| Comando | Qué hace | Ejemplo |
|---------|----------|---------|
| `head -n X` | Primeras X líneas | `cat log \| head -20` |
| `tail -n X` | Últimas X líneas | `history \| tail -10` |
| `grep "texto"` | Filtra líneas que contienen "texto" | `ps aux \| grep python` |
| `wc -l` | Cuenta líneas | `ls \| wc -l` |
| `sort` | Ordena líneas | `cat nombres.txt \| sort` |
| `uniq` | Elimina duplicados consecutivos | `sort lista.txt \| uniq` |
| `cut` | Extrae columnas | `cat datos.csv \| cut -d',' -f1` |
| `less` | Ver con scroll | `cat archivo_largo \| less` |

:::exercise{title="Practica pipes" difficulty="2"}

Ejecuta estos comandos y entiende qué hace cada pipe:

```bash
# 1. ¿Cuántos archivos/carpetas hay en /etc?
ls /etc | wc -l

# 2. ¿Cuáles son los últimos 5 comandos que ejecutaste?
history | tail -5

# 3. ¿Hay algún proceso de "bash" corriendo?
ps aux | grep bash

# 4. Muestra los archivos de tu home ordenados por tamaño
ls -la ~ | sort -k5 -n

# 5. Cuenta cuántas líneas tienen la palabra "error" en /var/log/syslog
cat /var/log/syslog | grep -i error | wc -l
```

:::

---

## Múltiples Comandos (sin pipe)

A diferencia del pipe que **conecta** salida→entrada, estos operadores **ejecutan comandos en secuencia**:

### Ejecutar en secuencia

```bash
# ; ejecuta todos, sin importar si fallan
comando1 ; comando2 ; comando3

# && ejecuta el siguiente SOLO si el anterior tuvo éxito
comando1 && comando2 && comando3

# || ejecuta el siguiente SOLO si el anterior falló
comando1 || comando2
```

### Diferencia entre `;`, `&&` y `|`

| Operador | Nombre | Qué hace |
|----------|--------|----------|
| `;` | Secuencia | Ejecuta uno después del otro |
| `&&` | AND | Ejecuta el siguiente si el anterior funcionó |
| `\|\|` | OR | Ejecuta el siguiente si el anterior falló |
| `\|` | Pipe | Conecta salida de uno con entrada de otro |

```bash
# ; - Ejecuta ambos sin importar qué pase
ls archivo_que_no_existe ; echo "esto se imprime igual"

# && - Solo ejecuta echo si ls funciona
ls archivo_que_no_existe && echo "esto NO se imprime"

# | - Pasa la salida de ls a grep
ls -la | grep ".txt"   # Filtra, no ejecuta en secuencia
```

### Ejemplos de `&&` y `||`

```bash
# Actualizar sistema (solo upgrade si update funciona)
sudo apt update && sudo apt upgrade

# Crear directorio e ir a él
mkdir proyecto && cd proyecto

# Intentar algo, si falla mostrar mensaje
comando_que_falla || echo "Falló el comando"

# Combinado: intenta, si falla avisa, si no continúa
make && echo "Compilación exitosa" || echo "Error al compilar"
```

---

## Tips de Productividad

### 1. Usa alias para comandos frecuentes

```bash
# Ver alias existentes
alias

# Crear un alias (temporal)
alias ll='ls -la'
alias ..='cd ..'
alias ...='cd ../..'
```

Para hacer alias permanentes, agrégalos a `~/.bashrc` o `~/.zshrc`.

### 2. Repite comandos con sudo

```bash
apt update
# Error: Permission denied

sudo !!
# Ejecuta: sudo apt update
```

### 3. Corrige errores rápido

```bash
# Escribiste mal
sl    # comando no encontrado

# Corrige y ejecuta
^sl^ls
# Ejecuta: ls
```

### 4. Usa variables para paths largos

```bash
# Guarda un path largo
PROYECTO="/home/usuario/Documents/trabajo/proyecto_importante"

# Úsalo
cd $PROYECTO
ls $PROYECTO
```

---

## Resumen de Atajos Clave

### Los 10 más importantes

| # | Atajo | Acción |
|---|-------|--------|
| 1 | `Tab` | Autocompletar |
| 2 | `Ctrl + C` | Cancelar |
| 3 | `Ctrl + L` | Limpiar pantalla |
| 4 | `Ctrl + R` | Buscar en historial |
| 5 | `↑` / `↓` | Navegar historial |
| 6 | `Ctrl + A` | Inicio de línea |
| 7 | `Ctrl + E` | Final de línea |
| 8 | `Ctrl + U` | Borrar hasta inicio |
| 9 | `Ctrl + W` | Borrar palabra |
| 10 | `!!` | Repetir último comando |

:::exercise{title="Práctica integral de atajos" difficulty="2"}

Realiza estas tareas usando atajos:

1. Escribe `echo "hola mundo"` y ejecútalo
2. Presiona `↑` para recuperarlo, cambia "hola" por "adios" usando `Ctrl + A` y `Ctrl + →`
3. Ejecuta `history | tail -5` para ver tus últimos comandos
4. Usa `Ctrl + R` y busca "echo"
5. Crea un alias: `alias c='clear'` y pruébalo

:::

---

---

## Ejercicios de Práctica

:::exercise{title="Speedrun de edición" difficulty="2"}

Practica estos atajos hasta que sean automáticos:

1. Escribe: `echo "Este es un comando muy largo que quiero editar rápidamente"`
2. Ve al inicio con `Ctrl + A`
3. Ve al final con `Ctrl + E`
4. Borra todo con `Ctrl + U`
5. Recupera con `Ctrl + Y`
6. Borra la última palabra con `Ctrl + W`

**Repite 5 veces** hasta que no tengas que pensar.

:::

:::exercise{title="Maestro del historial" difficulty="2"}

1. Ejecuta estos comandos:
   ```bash
   echo "primero"
   ls -la
   echo "segundo"
   pwd
   echo "tercero"
   ```

2. Usa `Ctrl + R` y busca "echo"
3. Presiona `Ctrl + R` otra vez para ir al siguiente resultado
4. Presiona `→` para editar el comando encontrado
5. Ejecuta `history | grep echo` para ver todos los echo

:::

:::exercise{title="Comodines en acción" difficulty="2"}

1. Crea archivos de prueba:
   ```bash
   mkdir ~/practica_comodines && cd ~/practica_comodines
   touch archivo{1..5}.txt documento{A,B,C}.md imagen{01..03}.png
   ```

2. Practica comodines:
   ```bash
   ls *.txt           # Solo .txt
   ls archivo?.txt    # archivo + 1 carácter + .txt
   ls documento[AB].md  # documentoA o documentoB
   ls imagen0[1-2].png  # imagen01 o imagen02
   ls *.{txt,md}      # .txt y .md
   ```

3. Limpia:
   ```bash
   cd ~ && rm -r practica_comodines
   ```

:::

:::exercise{title="Alias útiles" difficulty="3"}

Crea estos alias y pruébalos:

```bash
# Alias temporales (se pierden al cerrar terminal)
alias ll='ls -lah'
alias ..='cd ..'
alias ...='cd ../..'
alias cls='clear'
alias myip='curl -s ifconfig.me'
```

**Bonus:** Agrega los que más uses a `~/.bashrc` para hacerlos permanentes:
```bash
echo "alias ll='ls -lah'" >> ~/.bashrc
source ~/.bashrc
```

:::

---

## Prompts para LLM

:::prompt{title="Crear alias personalizados" for="ChatGPT/Claude"}

Soy estudiante de ciencia de datos y uso mucho la terminal. Dame 10 alias útiles para:
- Navegar más rápido
- Trabajar con Git
- Activar entornos virtuales de Python
- Ver logs y procesos

Explícame cómo agregarlos permanentemente a mi .bashrc

:::

:::prompt{title="Atajos que no funcionan" for="ChatGPT/Claude"}

Estoy en [macOS/Linux/WSL2] y el atajo [Ctrl + algo] no funciona. 

¿Por qué puede ser? ¿Hay un equivalente o configuración que deba cambiar?

:::

:::prompt{title="Personalizar el prompt" for="ChatGPT/Claude"}

Quiero personalizar cómo se ve mi prompt de bash/zsh. Actualmente se ve así:

```
usuario@computadora:~$
```

Quiero que muestre:
- El directorio actual de forma abreviada
- La rama de Git si estoy en un repositorio
- Colores para distinguir cada parte

Dame el código para agregar a mi .bashrc

:::

---

> **Recuerda:** En las siguientes secciones, cada vez que veas un recordatorio de atajos, **practícalo**. La única forma de aprenderlos es usándolos.
