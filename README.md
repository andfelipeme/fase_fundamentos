# Clase 1.1 - Entorno de trabajo en macOS

### ¿Qué es y para qué sirve?

- Terminal - "herdr": multiplexor de terminal y gestor de procesos diseñado específicamente para supervisar, organizar y ejecutar múltiples agentes de programación con inteligencia artificial.
- Homebrew: Instala herramientas en macOS con un comando.
- uv: Instala Python, crea proyectos, maneja dependencias y entornos virtuales. Reemplaza pip, venv y pyenv.
- VSCode + Ruff: el IDE y la herramienta que revisa errores y da formato al código.
- Claude Code: mi agente de IA en herdr, y mi instructor.

### Comandos básicos

- pwd --> Dónde estoy parado
- ls -la --> Qué hay en esta ruta
- cd --> moverme por mis archivos
- mkdir -p --> crear carpeta
- touch --> crear un archivo
- open . --> abre la ruta actual en Finder
- mv --> renombrar un archivo
- rm --> borrar un archivo
- rm -r --> borrar una carpeta
- cat y less --> leer archivos
- which --> dice dónde está un programa

### Iniciar un proyecto con uv

```bash
uv init caja-cli --python 3.14
cd caja-cli
uv add --dev ruff pytest
uv run caja-cli             # Imprime: Hello from caja-cli!
uv run ruff check .         # busca errores
uv run ruff format .        # da formato
code .                      # abre el proyecto en VSCode
```

### Práctica

1. Crear el proyecto caja-cli con los comandos de arriba y ejecutarlo
2. Solo con la terminal: crear las carpetas notas/ y datos/
3. En VSCode instalar las extensiones de Python y Ruff, y activar el formato al guardar con Ruff.
4. Escribe el CLAUDE.md y pídele a Claude Code que te explique pyproject.toml línea por línea.
5. Reto sin IA: explica en tus apuntes qué hace cada archivo que creó uv init.

### Estructura de uv init 

- .venv --> carpeta donde se guardan todas las dependencias y librerías que se instalen. "Entorno virtual"
- src --> carpeta donde se almacena el código fuente.
- .python-version --> archivo que indica a uv qué versión de Python usar para crear el .venv
- pyproject.toml --> archivo de configuración del proyecto.
- README.md --> Documentación de nuestro proyecto.
- uv.lock --> archivo que guarda las versiones de nuestras dependencias y librerías. No se debe editar.
- .gitignore --> se indican los archivos que no queremos ni debemos subir a git
