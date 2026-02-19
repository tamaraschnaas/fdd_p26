# Bash como Lenguaje de Programación

![Bash lenguaje - Lain en su habitación rodeada de código](./images/01_bash_lain_code.png)

Hasta ahora has usado Bash para ejecutar comandos uno por uno. Pero Bash es mucho más que eso: **es un lenguaje de programación completo**.

## ¿Qué Puede Hacer Bash?

Como cualquier lenguaje de programación, Bash puede:

| Característica | ¿Bash lo tiene? | Ejemplo |
|----------------|-----------------|---------|
| Variables | Sí | `nombre="Juan"` |
| Operaciones matemáticas | Sí | `$((5 + 3))` |
| Condicionales (if/else) | Sí | `if [ -f archivo ]; then ...` |
| Bucles (loops) | Sí | `for i in 1 2 3; do ...` |
| Funciones | Sí | `function saludo() { ... }` |
| Entrada/Salida | Sí | `read`, `echo`, redirección |

---

## Dos Modos de Usar Bash

### 1. Modo Interactivo (Lo que conoces)

Escribes comandos uno por uno y ves los resultados inmediatamente:

```bash
$ echo "Hola"
Hola
$ date
Mon Jan 27 10:30:00 CST 2026
```

**Usaremos este modo para aprender** - es más fácil experimentar y ver qué pasa.

### 2. Modo Script (Lo veremos al final)

Escribes múltiples comandos en un archivo y los ejecutas todos juntos:

```bash
#!/bin/bash
# Este es un script
echo "Hola"
date
echo "Adiós"
```

**Guardaremos los scripts para el final** - primero aprende los conceptos.

---

## Comentarios en Bash

Todo lo que viene después de `#` es un comentario (Bash lo ignora):

```bash
# Esto es un comentario - Bash lo ignora
echo "Hola"  # Esto también es un comentario

# Los comentarios sirven para:
# - Explicar qué hace tu código
# - Desactivar código temporalmente
# - Documentar
```

:::exercise{title="Prueba los comentarios" difficulty="1"}

Ejecuta en tu terminal:

```bash
# Esto no hace nada
echo "Esto sí se ejecuta"  # pero esto es comentario
# echo "Esto NO se ejecuta"
```

¿Cuántas líneas de output ves?

:::

---

## Expresiones en la Terminal

Puedes hacer más que ejecutar comandos. Bash entiende **expresiones** especiales que se reconocen por el símbolo `$`.

### La Sintaxis del `$`

El símbolo `$` le dice a Bash: **"esto no es texto literal, evalúalo primero"**.

| Sintaxis | Qué significa | Ejemplo |
|----------|---------------|---------|
| `$variable` | "Dame el valor de esta variable" | `$nombre` → el valor guardado |
| `$(comando)` | "Ejecuta esto y dame el resultado" | `$(date)` → la fecha actual |
| `$((matemáticas))` | "Calcula esta expresión" | `$((2+2))` → 4 |

> **Sin `$`** = texto literal. **Con `$`** = Bash lo procesa primero.

---

### Matemáticas: `$(( ))`

La sintaxis `$(( expresión ))` evalúa matemáticas:

```
$((  expresión  ))
 │       │
 │       └── operación matemática (suma, resta, etc.)
 └── doble paréntesis = modo matemático
```

```bash
echo $((5 + 3))
# 8  ← Bash calcula 5+3 y devuelve el resultado

echo $((10 * 2))
# 20

echo $((100 / 4))
# 25
```

**Operadores disponibles:**
- `+` suma, `-` resta, `*` multiplicación
- `/` división (entera), `%` residuo (módulo)
- `**` potencia (ej: `$((2**3))` = 8)

---

### Variables: `$nombre`

Cuando escribes `$` seguido de un nombre, Bash busca una variable con ese nombre y la reemplaza por su valor:

```
$nombre
 │  │
 │  └── nombre de la variable
 └── "dame el valor de..."
```

```bash
# 1. Crear variable (guardar un valor)
nombre="María"

# 2. Leer variable (obtener el valor)
echo $nombre
# María  ← Bash reemplaza $nombre por "María"

# 3. Usar dentro de texto
echo "Hola, $nombre"
# Hola, María  ← $nombre se expande dentro de las comillas dobles
```

**Importante:** Para **crear** una variable NO usas `$`. Para **leer** una variable SÍ usas `$`.

---

### Sustitución de comandos: `$(comando)`

La sintaxis `$(comando)` ejecuta un comando y captura su salida:

```
$(  comando  )
 │     │
 │     └── cualquier comando válido
 └── "ejecuta y dame el resultado"
```

```bash
# Sin $() - solo imprime la palabra "date"
echo date
# date

# Con $() - ejecuta date y usa su resultado
echo $(date)
# Mon Jan 27 10:30:00 CST 2026

# Guardar en variable
fecha=$(date)
echo "La fecha es: $fecha"
# La fecha es: Mon Jan 27 10:30:00 CST 2026
```

**¿Qué pasa paso a paso?**
1. Bash ve `$(date)`
2. Ejecuta el comando `date`
3. Captura el output: "Mon Jan 27 10:30:00 CST 2026"
4. Reemplaza `$(date)` por ese texto
5. Continúa con el resto del comando

:::exercise{title="Expresiones básicas" difficulty="1"}

Ejecuta cada línea en tu terminal:

```bash
# 1. Matemáticas
echo $((2 + 2))
echo $((10 - 3))
echo $((4 * 5))

# 2. Una variable
saludo="Hola mundo"
echo $saludo

# 3. Capturar un comando
usuario=$(whoami)
echo "Soy $usuario"
```

:::

---

## El Prompt: Tu Indicador

Cuando ves algo como esto:

```
usuario@computadora:~$
```

El `$` al final indica que estás en modo usuario normal. Si ves `#`, estás como root (administrador).

**Todo lo que escribes después del `$` es interpretado por Bash.**

---

## ¿Por Qué Aprender Bash como Lenguaje?

1. **Automatización**: Tareas repetitivas se vuelven scripts de un clic
2. **Servidores**: La mayoría de servidores Linux se administran con Bash
3. **DevOps/CI-CD**: Pipelines de deployment usan scripts Bash
4. **Eficiencia**: Combinar comandos es más rápido que ejecutarlos uno por uno
5. **Portabilidad**: Bash está en casi cualquier sistema Unix/Linux

---

## Ejercicios de Práctica

:::exercise{title="Tu primera sesión de Bash" difficulty="1"}

Abre tu terminal y ejecuta:

```bash
# 1. Verifica tu shell
echo $SHELL

# 2. Un cálculo
echo "2 + 2 = $((2 + 2))"

# 3. Tu usuario
echo "Usuario: $(whoami)"

# 4. Directorio actual
echo "Estás en: $(pwd)"

# 5. La fecha
echo "Fecha: $(date)"
```

:::

:::exercise{title="Crear variables y usarlas" difficulty="1"}

```bash
# Crea estas variables (¡sin espacios alrededor del =!)
mi_nombre="Tu Nombre"
mi_edad=25
mi_ciudad="CDMX"

# Ahora úsalas
echo "Me llamo $mi_nombre"
echo "Tengo $mi_edad años"
echo "Vivo en $mi_ciudad"

# Combínalas
echo "$mi_nombre tiene $mi_edad años y vive en $mi_ciudad"
```

:::

:::exercise{title="Matemáticas con Bash" difficulty="2"}

```bash
# Usa $(( )) para calcular
a=10
b=3

echo "Suma: $((a + b))"
echo "Resta: $((a - b))"
echo "Multiplicación: $((a * b))"
echo "División: $((a / b))"
echo "Residuo: $((a % b))"

# ¿Qué resultado da cada uno?
```

:::

---

## Errores Comunes

### Error: Espacios en asignación

```bash
# MAL - causa error
nombre = "Juan"
# bash: nombre: command not found

# BIEN - sin espacios
nombre="Juan"
```

### Error: Olvidar el $

```bash
nombre="Juan"

# MAL - imprime literal "nombre"
echo nombre
# nombre

# BIEN - imprime el valor
echo $nombre
# Juan
```

---

## Resumen

| Concepto | Descripción |
|----------|-------------|
| Modo interactivo | Ejecutar comandos uno por uno |
| Modo script | Guardar comandos en un archivo |
| Comentarios | `# texto` - Bash los ignora |
| Variables | `VAR=valor` (sin espacios) |
| Leer variable | `$VAR` o `${VAR}` |
| Matemáticas | `$((expresión))` |
| Capturar comando | `$(comando)` |

---

> **Siguiente:** Ahora que sabes que Bash es un lenguaje, vamos a profundizar en las **variables**.
