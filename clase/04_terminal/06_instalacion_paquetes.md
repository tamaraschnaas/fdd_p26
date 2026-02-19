# Instalación de Paquetes

![Instalación de paquetes - Frieren invocando items de su bolsa mágica](./images/06_packages_frieren_bag.png)

Cómo instalar software en tu sistema usando gestores de paquetes.

---

## ¿Qué es un Gestor de Paquetes?

Un gestor de paquetes es como una "tienda de apps" para la terminal. Se encarga de:
- Descargar software
- Instalar dependencias automáticamente
- Actualizar programas
- Desinstalar limpiamente

| Sistema | Gestor de Paquetes |
|---------|-------------------|
| Ubuntu/Debian/WSL2 | `apt` |
| macOS | `brew` (Homebrew) |
| Fedora | `dnf` |
| Arch Linux | `pacman` |
| Python | `pip` |
| Node.js | `npm` |

---

![Instalando htop - Stark invocando un monitor de sistema mágico](./images/06_htop_stark_monitor.png)

## APT (Ubuntu/Debian/WSL2)

**A**dvanced **P**ackage **T**ool - el gestor de paquetes de Debian y derivados.

### Actualizar lista de paquetes

Siempre hazlo antes de instalar algo nuevo:

```bash
sudo apt update
```

### Actualizar paquetes instalados

```bash
# Ver qué se actualizará
sudo apt upgrade

# Actualizar todo automáticamente
sudo apt upgrade -y
```

### Instalar paquetes

```bash
# Instalar un paquete
sudo apt install nombre_paquete

# Instalar sin preguntar
sudo apt install -y nombre_paquete

# Instalar varios
sudo apt install paquete1 paquete2 paquete3
```

### Paquetes útiles para desarrollo

```bash
# Herramientas básicas
sudo apt install -y git curl wget

# Python y pip
sudo apt install -y python3 python3-pip python3-venv

# Compiladores y herramientas de build
sudo apt install -y build-essential

# Editor de texto
sudo apt install -y nano vim

# Herramientas de red
sudo apt install -y net-tools

# Monitor de sistema mejorado
sudo apt install -y htop

# Herramienta para ver info del sistema
sudo apt install -y neofetch
```

### Buscar paquetes

```bash
# Buscar un paquete
apt search nombre

# Ver información de un paquete
apt show nombre_paquete
```

### Desinstalar

```bash
# Desinstalar paquete
sudo apt remove nombre_paquete

# Desinstalar y eliminar configuración
sudo apt purge nombre_paquete

# Eliminar paquetes huérfanos
sudo apt autoremove
```

### Comandos comunes combinados

```bash
# Actualización completa del sistema
sudo apt update && sudo apt upgrade -y

# Instalar después de actualizar
sudo apt update && sudo apt install -y nuevo_paquete
```

---

## Homebrew (macOS)

Homebrew es el gestor de paquetes no oficial de macOS.

### Instalar Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Comandos básicos

```bash
# Actualizar Homebrew
brew update

# Actualizar paquetes
brew upgrade

# Instalar paquete
brew install nombre

# Buscar paquete
brew search nombre

# Ver información
brew info nombre

# Desinstalar
brew uninstall nombre

# Limpiar versiones antiguas
brew cleanup
```

### Paquetes útiles

```bash
# Herramientas básicas
brew install git curl wget

# Python
brew install python

# Node.js
brew install node

# Bases de datos
brew install postgresql
brew install mysql

# Herramientas útiles
brew install htop
brew install tree
brew install jq
```

### Casks (aplicaciones con interfaz gráfica)

```bash
# Instalar aplicaciones GUI
brew install --cask visual-studio-code
brew install --cask docker
brew install --cask google-chrome
```

---

![pip Python - Serpiente mágica (Python) entregando paquetes](./images/06_pip_python_snake.png)

## pip (Python)

`pip` es el gestor de paquetes de Python.

### Verificar instalación

```bash
# Python 3
pip3 --version
# o
python3 -m pip --version
```

### Instalar paquetes

```bash
# Instalar paquete
pip3 install nombre_paquete

# Instalar versión específica
pip3 install paquete==1.2.3

# Instalar desde requirements.txt
pip3 install -r requirements.txt
```

### Paquetes útiles para data science

```bash
# Ciencia de datos
pip3 install numpy pandas matplotlib

# Machine Learning
pip3 install scikit-learn

# Jupyter
pip3 install jupyter jupyterlab

# Formateo de código
pip3 install black flake8

# Manejo de APIs
pip3 install requests
```

### Ver paquetes instalados

```bash
# Listar paquetes
pip3 list

# Ver info de un paquete
pip3 show nombre_paquete

# Guardar dependencias
pip3 freeze > requirements.txt
```

### Desinstalar

```bash
pip3 uninstall nombre_paquete
```

### Entornos virtuales (recomendado)

Es mejor práctica usar entornos virtuales para proyectos:

```bash
# Crear entorno virtual
python3 -m venv mi_entorno

# Activar entorno (Linux/macOS)
source mi_entorno/bin/activate

# Activar entorno (Windows en WSL2)
source mi_entorno/bin/activate

# Ver que estás en el entorno (aparece el nombre)
# (mi_entorno) usuario@pc:~$

# Instalar paquetes (solo afecta este entorno)
pip install pandas numpy

# Desactivar entorno
deactivate
```

---

## npm (Node.js/JavaScript)

### Instalar Node.js

```bash
# Ubuntu/WSL2
sudo apt install nodejs npm

# macOS
brew install node
```

### Comandos básicos

```bash
# Instalar paquete local (proyecto)
npm install paquete

# Instalar paquete global
npm install -g paquete

# Instalar dependencias de proyecto
npm install

# Desinstalar
npm uninstall paquete
```

---

## Consejos de Seguridad

### 1. Entiende qué estás instalando

```bash
# Antes de instalar, revisa qué hace
apt show paquete
brew info paquete
pip3 show paquete
```

### 2. Usa fuentes oficiales

- Evita agregar repositorios de terceros desconocidos
- Para Python, usa PyPI (pip install)
- Para Node, usa npm oficial

### 3. Mantén tu sistema actualizado

```bash
# Hazlo regularmente
# Ubuntu/WSL2
sudo apt update && sudo apt upgrade -y

# macOS
brew update && brew upgrade
```

### 4. No uses `sudo pip install`

```bash
# MAL - puede romper el sistema
sudo pip3 install paquete

# BIEN - usa entornos virtuales
python3 -m venv venv
source venv/bin/activate
pip install paquete
```

---

## Resumen

### Ubuntu/WSL2 (apt)

```bash
sudo apt update              # Actualizar lista
sudo apt upgrade             # Actualizar paquetes
sudo apt install paquete     # Instalar
sudo apt remove paquete      # Desinstalar
apt search nombre            # Buscar
```

### macOS (brew)

```bash
brew update                  # Actualizar Homebrew
brew upgrade                 # Actualizar paquetes
brew install paquete         # Instalar
brew uninstall paquete       # Desinstalar
brew search nombre           # Buscar
```

### Python (pip)

```bash
pip3 install paquete         # Instalar
pip3 uninstall paquete       # Desinstalar
pip3 list                    # Ver instalados
pip3 freeze > requirements.txt  # Guardar deps
```

---

## Ejercicio Práctico

:::exercise{title="Configurar entorno de desarrollo" difficulty="2"}

Configura tu entorno para desarrollo en Python:

1. Actualiza tu sistema:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. Instala herramientas básicas:
   ```bash
   sudo apt install -y git python3 python3-pip python3-venv
   ```

3. Crea un entorno virtual:
   ```bash
   mkdir mi_proyecto && cd mi_proyecto
   python3 -m venv venv
   source venv/bin/activate
   ```

4. Instala paquetes de ciencia de datos:
   ```bash
   pip install numpy pandas matplotlib jupyter
   ```

5. Guarda las dependencias:
   ```bash
   pip freeze > requirements.txt
   ```

6. Verifica:
   ```bash
   python3 -c "import pandas; print(pandas.__version__)"
   ```

:::

---

---

## Más Ejercicios

:::exercise{title="Explorar paquetes instalados" difficulty="1"}

Investiga qué tienes instalado:

```bash
# Ver todos los paquetes apt (puede ser largo)
apt list --installed | head -30

# Buscar paquetes específicos
apt list --installed | grep python

# Ver información de un paquete
apt show python3

# Ver de dónde viene un comando
which python3
which git
```

:::

:::exercise{title="Instalar herramienta útil" difficulty="2"}

Instala `tldr` - páginas de manual simplificadas:

```bash
# Opción 1: Con pip
pip3 install tldr

# Opción 2: Con npm (si tienes Node)
npm install -g tldr

# Uso
tldr ls
tldr tar
tldr git commit
```

**Compara:** `man tar` vs `tldr tar`

:::

:::exercise{title="Crear proyecto Python desde cero" difficulty="3"}

Practica el flujo completo:

```bash
# 1. Crear directorio del proyecto
mkdir mi_primer_proyecto && cd mi_primer_proyecto

# 2. Crear entorno virtual
python3 -m venv venv

# 3. Activar entorno
source venv/bin/activate

# 4. Verificar que estás en el entorno
which python
# Debería mostrar: .../mi_primer_proyecto/venv/bin/python

# 5. Instalar paquetes
pip install requests pandas

# 6. Guardar dependencias
pip freeze > requirements.txt

# 7. Crear un script de prueba
cat << 'EOF' > main.py
import requests
import pandas as pd

print("¡Instalación exitosa!")
print(f"Pandas version: {pd.__version__}")
EOF

# 8. Ejecutar
python main.py

# 9. Desactivar cuando termines
deactivate
```

:::

:::exercise{title="Simular error y resolverlo" difficulty="2"}

Practica resolver problemas comunes:

```bash
# Error 1: Falta sudo
apt install htop
# Solución:
sudo apt install htop

# Error 2: Paquete no encontrado
sudo apt install paquete_que_no_existe
# Solución: buscar nombre correcto
apt search htop

# Error 3: pip sin permisos
pip install pandas  # puede fallar
# Solución: usar entorno virtual o --user
pip install --user pandas
```

:::

---

## Prompts para LLM

:::prompt{title="Instalar software específico" for="ChatGPT/Claude"}

Quiero instalar [nombre del software] en [Ubuntu/macOS/WSL2].

1. ¿Cuál es la mejor forma de instalarlo?
2. ¿Hay dependencias que necesite instalar primero?
3. ¿Cómo verifico que se instaló correctamente?
4. ¿Cómo lo configuro básicamente?

:::

:::prompt{title="Resolver error de instalación" for="ChatGPT/Claude"}

Estoy intentando instalar [paquete] con [apt/pip/brew] y obtengo este error:

```
[pega el error completo]
```

Mi sistema es [Ubuntu/macOS/WSL2] versión [X].

¿Cómo soluciono este error?

:::

:::prompt{title="Configurar entorno de desarrollo" for="ChatGPT/Claude"}

Soy estudiante de [ciencia de datos/desarrollo web/etc] y estoy configurando mi computadora con [Ubuntu/macOS/WSL2].

Dame una lista de herramientas esenciales que debería instalar, con los comandos exactos para cada una. Incluye:
- Lenguajes de programación
- Editores/IDEs
- Herramientas de control de versiones
- Utilidades de terminal

:::

:::prompt{title="Entender gestor de paquetes" for="ChatGPT/Claude"}

Explícame las diferencias entre:
- apt (Ubuntu/Debian)
- brew (macOS)
- pip (Python)
- npm (Node.js)
- conda (Anaconda)

¿Cuándo uso cada uno? ¿Pueden coexistir? ¿Pueden causar conflictos?

:::

:::prompt{title="Limpiar sistema" for="ChatGPT/Claude"}

Mi sistema [Ubuntu/WSL2/macOS] está usando mucho espacio en disco.

Dame comandos para:
1. Ver qué está usando más espacio
2. Limpiar paquetes y caché de apt/brew
3. Limpiar paquetes de pip no utilizados
4. Encontrar y eliminar archivos grandes innecesarios

:::

---

## Tarea del Módulo

:::exercise{title="Preparar tu sistema para el curso" difficulty="2"}

Asegúrate de tener estas herramientas instaladas (las necesitarás más adelante):

**Ubuntu/WSL2:**
```bash
sudo apt update
sudo apt install -y git curl wget python3 python3-pip
```

**macOS:**
```bash
brew install git curl wget python3
```

**Verificación:**
```bash
git --version
python3 --version
ssh -V  # Necesario para Bandit
```

:::

---

## Recordatorio de Atajos

| Atajo | Úsalo para |
|-------|------------|
| `Tab` | Autocompletar nombres de paquetes |
| `Ctrl + C` | Cancelar instalación si algo sale mal |
| `↑` | Repetir comando de instalación si falla |
| `!!` | Repetir último comando con `sudo !!` |
