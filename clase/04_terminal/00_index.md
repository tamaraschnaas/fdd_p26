# Módulo 4: La Terminal

![Terminal - Lain en su habitación con múltiples monitores](./images/00_terminal_lain_monitors.png)

:::homework{id="4.0" title="Videos SSH" due="2026-01-27" points="10"}

Ver los siguientes videos sobre SSH (se preguntará al respecto en clase), necesitaras instalar y configurar to ssh local apra jugar bandit. **Si tienes wsl2 hazlo en wsl2 no powershell**:

1. [Video 1: Introducción a SSH](https://www.youtube.com/watch?v=5JvLV2-ngCI)
2. [ the basics of secure shell (ssh) ](https://www.youtube.com/watch?v=WwGRGfLy6q8&embeds_referring_euri=https%3A%2F%2Fdocs.google.com%2F&embeds_referring_origin=https%3A%2F%2Fdocs.google.com&source_ve_path=Mjg2NjY)

**Se hará quiz en clase sobre el contenido de los videos.**

:::

:::homework{id="4.1" title="Bandit OverTheWire" due="2026-01-27" points="15"}

**URL:** [https://overthewire.org/wargames/bandit/bandit0.html](https://overthewire.org/wargames/bandit/bandit0.html)

Bandit es un juego de "wargames" para aprender comandos de terminal. Tu objetivo:

**Completar los niveles 0 al 5** (tener la contraseña para entrar al nivel 6).

### REGLA PRINCIPAL: NO USES LLMs (ChatGPT, Claude, Gemini, etc.)

Esta tarea es para que aprendas a buscar información **a la antigua**. Queremos que:

1. **Pienses** qué necesitas hacer
2. **Busques** cómo funciona el comando (no la solución)
3. **Experimentes** en la terminal

### Recursos permitidos:

| Recurso | Para qué usarlo |
|---------|-----------------|
| `man comando` | Manual del sistema en tu terminal |
| `comando --help` | Ayuda rápida |
| **Google** | Buscar "how to [cosa] in linux", NO "bandit level X solution" |
| **Stack Overflow** | Buscar cómo funcionan comandos |
| **explainshell.com** | Entender partes de un comando |
| **tldr.sh** | Ejemplos prácticos de comandos |
| **Las pistas de Bandit** | Cada nivel dice "Commands you may need" |

### NO permitido:

- ChatGPT, Claude, Gemini, Copilot, o cualquier LLM
- Buscar "bandit level 0 solution", "bandit walkthrough", etc.
- Copiar soluciones de otros

### Entrega:

**Archivo `bandit_solucion.txt`** con este formato para CADA nivel (0-5):

```
=== NIVEL X ===
1. ¿Qué necesito hacer? (en tus palabras)
2. ¿Qué comando(s) creo que necesito?
3. ¿Dónde busqué información? (link o man page)
4. Comandos que ejecuté:
   $ comando1
   $ comando2
5. ¿Funcionó? Si no, ¿qué ajusté?
```

**Screenshot** mostrando que entraste al nivel 6.

**Tu nombre y clave** en el archivo.

> **Nota:** La siguiente clase algunos pasarán a explicar cómo resolvieron un nivel. Debes poder explicar tu proceso de pensamiento, no solo los comandos.

:::


La terminal es la interfaz más poderosa para interactuar con tu computadora. Dominarla es esencial para cualquier desarrollador, científico de datos o ingeniero de IA.

## ¿Por qué aprender la terminal?

- **Eficiencia:** Muchas tareas son más rápidas por terminal que por interfaz gráfica
- **Automatización:** Puedes crear scripts para tareas repetitivas
- **Acceso remoto:** Servidores y servicios cloud solo ofrecen acceso por terminal
- **Herramientas de desarrollo:** Git, Docker, Python, y la mayoría de herramientas modernas se usan desde la terminal

## Contenido

1. **[Conceptos Básicos](./01_conceptos_basicos.md)**
   - ¿Qué es la terminal? ¿Qué es el shell?
   - Anatomía de un comando
   - Tu primera interacción

2. **[Navegación y Rutas](./02_navegacion.md)**
   - El sistema de archivos
   - Rutas absolutas vs relativas
   - El directorio home (`~`)
   - Comandos: `pwd`, `ls`, `cd`

3. **[Atajos y Productividad](./03_atajos_tips.md)**
   - Atajos de teclado esenciales
   - Historial de comandos
   - Tab completion
   - *Aprende estos temprano para practicarlos en las siguientes secciones*

4. **[Manipulación de Archivos](./04_manipulacion_archivos.md)**
   - Crear archivos y directorios
   - Copiar, mover y renombrar
   - Eliminar archivos (con cuidado)
   - Ver contenido de archivos

5. **[Comandos Útiles](./05_comandos_utiles.md)**
   - Búsqueda y filtrado
   - Redirección y pipes
   - Permisos de archivos
   - Procesos

6. **[Instalación de Paquetes](./06_instalacion_paquetes.md)**
   - Gestores de paquetes del sistema (`apt`, `brew`)
   - Python y `pip`
   - Actualizar tu sistema

---

## Antes de empezar

Asegúrate de tener acceso a una terminal:

| Sistema Operativo | Cómo abrir la terminal |
|-------------------|------------------------|
| **macOS** | `Cmd + Espacio` → escribir "Terminal" |
| **Linux** | `Ctrl + Alt + T` |
| **Windows (WSL2)** | Buscar "Ubuntu" en el menú inicio |
| **Windows (PowerShell)** | `Win + X` → Terminal |

> **Nota:** Para este curso usamos terminales Unix (Linux, macOS, WSL2). Los comandos de PowerShell son diferentes.

---

![Búsqueda de información - Frieren leyendo grimorios antiguos](./images/00_search_frieren_books.png)

## Cómo buscar información (sin LLMs)

Aprende a buscar información como se hacía antes de ChatGPT. Estas habilidades son importantes porque:
- Los LLMs pueden estar equivocados
- En el trabajo real necesitas verificar información
- Entiendes mejor cuando buscas tú mismo

### En tu terminal:

```bash
# Manual completo de un comando
man ls

# Ayuda rápida
ls --help

# Buscar comandos por descripción
apropos "search file"

# Descripción corta
whatis grep

# Desde la terminal (requiere internet)
curl cheat.sh/tar
```

### En internet:

| Qué buscar | Dónde |
|------------|-------|
| "how to list hidden files linux" | Google |
| "linux find file by name" | Stack Overflow |
| Entender `ls -la` | explainshell.com |
| Ejemplos de `grep` | tldr.sh o devhints.io |
| Referencia completa | ss64.com/bash |

### Ejemplo de búsqueda correcta:

**MAL:** "bandit level 1 solution" ❌

**BIEN:** "linux how to read file with special characters in name" ✓

**MAL:** "overthewire bandit walkthrough" ❌

**BIEN:** "man cat" en tu terminal ✓

---

## Ejercicio de Inicio Rápido

:::exercise{title="Verificar tu terminal" difficulty="1"}

Antes de continuar, asegúrate de que tu terminal funciona:

```bash
# 1. Abre tu terminal

# 2. Ejecuta estos comandos y verifica que no hay errores:
whoami
pwd
ls
echo "¡Mi terminal funciona!"

# 3. Si todo funciona, continúa al siguiente tema
```

Si algún comando falla, revisa el [Módulo A.3: Configuración del Sistema Operativo](../a_stack/03_os_setup/00_index.md).

:::

---

## Prompt de Inicio

:::prompt{title="Ayuda si tu terminal no funciona" for="ChatGPT/Claude"}

Estoy intentando usar la terminal en [Windows WSL2 / macOS / Linux] y tengo este problema:

[describe tu problema]

Cuando ejecuto [comando], obtengo:

```
[pega el error o comportamiento inesperado]
```

¿Cómo puedo solucionarlo?

:::
