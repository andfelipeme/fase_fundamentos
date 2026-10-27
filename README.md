# Clase 1.1 - Entorno de trabajo en macOS

### ¿Qué es y para qué sirve?

- Terminal - "herdr": multiplexor de terminal y gestor de procesos diseñado específicamente para supervisar, organizar y ejecutar múltiples agentes de programación con inteligencia artificial.
- Homebrew: Instala herramientas en macOS con un comando.
- uv: Instala Python, crea proyectos, maneja dependencias y entornos virtuales. Reemplaza pip, venv y pyenv. Escrito en Rust.
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

# Clase 1.2 - Git y GitHub

### Qué es y para qué sirve?

- Git --> Guarda el historial de tu código en tu computador. Manejando versiones de nuesstro proyecto.
- GitHub --> guarda los repositorios en la nube, sirve de portafolio y ejecuta pruebas automáticas.

### Importante

- git add --> Prepara lo que se va subir
- git commit --> los guarda en el historial
- Un commit debe ser un cambio con sentido. feat: función nueva, fix: correción, docs: documentación, test: testeo, refactor: refactorización.
- Rama --> línea de trabajo aparte. main es la principal, pero para manejar mejor nuestro historial se puede trabajar con distintas ramas que combinen con el cambio que se va hacer.
- Nunca subas secretos o archivos privados. Para eso usa .gitignore

### Codigo

```bash
#   Configuración (una sola vez)
git config --global user.name "Tu nombre"
git config --global user.email "Tu correo"
git config --global init.defaultBranch main
gh auth login

# Primer commit y repositorio en Github usando gh
cd caja-cli
git status
git add .
git commit -m "feat: crear proyecto caja-cli con uv"
gh repo create caja-cli --private --source=. --push

# Primer commit y creación de repo
cd caja-cli
git status
git add .
git commit -m "..."
git branch -M main
git remote add origin url
git push -u origin main

# Flujo de trabajo con ramma y pull request
git switch -c feature/precio-por-peso
# ...se escribe codigo...
git diff                     # Qué cambio?
git add -p o git add .       # agrega cambio por cambio revisando (-p)
git commit -m "..."
git push -u origin feat/precio-por-peeso
gh pr create --fill
gh pr merge --squash --delete-branch
git switch main
git pull

# Consultar y deshacer
git log --oneline --graph           # historial
git restore archivo.py              # descarta cambios sin guardar de un archivo
git revert <id-del=commit>          # deshace un commit creando otro (seguro)
```

### Practica

1. Sube caja-cli a Github como repositorio privado
2. Haz 5 commits pequeños con el formato feat: / fix: / docs: / chore: 
3. Crea un conflicto a propósito: cambia la misma línea de README en dos ramas, fusionalas y resuélvelo a mano
4. Abre un pull request con gh pr create, revisalo en la web y fusionalo.
5. Reto sin IA: dibuja en tus nota el historial de ramas que muestra ```git log --oneline --graph```


### 5.
```bash
git log --oneline --graph
*   69fbf33 (HEAD -> main, origin/main, origin/HEAD) fix: Se resuelve conflicto entre fix/a y fix/b
|\
| * 75576b4 (fix/b) feat: Modificacion del titulo B
* | 4c06303 (fix/a) feat: Modicacion del titulo A
|/
* 4f4a09f docs: Se agraga como se trabaja con el historial en git (#1)
* 176a751 feat: Se hace Hello World
* 27f4940 docs: Se agrega como se crea el proyecto con uv
* 5a34dd8 chore: Se agrega .gitignore y se deja de rastrear __pycache__
* f8337b1 feat: crear proyecto caja-cli con uv
```

# Clase 1.3 Python 1 - tipos, decimal y funciones

### Qué es y para qué sirce?

-  Tipos básicos: (int, float, bool, str, None) --> La materia prima de cualquier programa.
- Decimal: números exactos para dinero y pesos. **float** acumula errores de redondeo y pues eso causa descuadres.
- Funciones con tipos: bloques reutilizaables que declaran qué reciben y qué devuelven.



```python
from decimal import ROUND_HALF_UP, Decimal


def main() -> None:
    def precio_venta(precio_kg: Decimal, peso_kg: Decimal) -> Decimal:
        total = precio_kg * peso_kg
        return total.quantize(Decimal("1"), rounding=ROUND_HALF_UP)

    def formato_pesos(valor: Decimal) -> str:
        if valor is None:
            return "$0"
        else:
            return "$" + f"{valor:,.0f}".replace(",", ".")

    def descuento(tipo_cliente: str) -> Decimal:
        match tipo_cliente:
            case "mayorista":
                return Decimal("0.10")
            case "frecuente":
                return Decimal("0.05")
            case _:
                return Decimal("0")

    def aplicar_descuento(total, porcentaje) -> Decimal:
        return total * (1 - porcentaje)

    def calcular_cambio(total, pago) -> Decimal | None:
        if pago < total:
            return None
        else:
            cambio = pago - total
            return cambio.quantize(Decimal("1"), rounding=ROUND_HALF_UP)

    while True:
        try:
            precio_kg = Decimal(input("Ingrese el precio por kg: ").replace(",", "."))
            if precio_kg > 0:
                break
            elif precio_kg <= 0:
                print("El precio por kg debe ser un valor positivo.")
                continue
        except Exception:
            print("Ingrese un valor válido para el precio por kg, por ejemplo: 10000")

    peso_total = Decimal("0")
    venta_total = Decimal("0")
    contador_ventas = 0

    while True:
        try:
            entrada = input("Ingrese el peso en kg (o 'fin' para terminar): ").strip()
            # strip() elimina los espacios en blanco al inicio y al final de la cadena
            if entrada.lower() == "fin":
                print("------ Resumen de ventas ------")
                print(f"Se realizaron {contador_ventas} ventas.")
                print(f"Se vendieron {peso_total} kg de producto.")
                print(f"Venta total: {formato_pesos(venta_total)}")
                break

            peso_kg = Decimal(entrada.replace(",", "."))

            if peso_kg <= 0:
                print("El peso debe ser un valor positivo.")
                continue
            elif peso_kg > 0:
                total = precio_venta(precio_kg, peso_kg)
                peso_total += peso_kg
                venta_total += total
                contador_ventas += 1
                print(f"Se hizo una venta de {formato_pesos(total)}")

        except Exception:
            print("Ingrese un valor numérico válido para el peso, por ejemplo: 1.5")

    tipo_cli = input("Ingrese el tipo de cliente (mayorista/frecuente/otro): ").lower()
    total_con_descuento = aplicar_descuento(venta_total, descuento(tipo_cli))
    print(f"Total con descuento: {formato_pesos(total_con_descuento)}")  # Total con des
    devuelta = calcular_cambio(venta_total, Decimal("40000"))  # ALmacena el cambio
    print(f"Cambio: {formato_pesos(devuelta)}")  # Devuelve el cambio en pesos cop

```

Práctica:

1. Escribe la función aplicar_descuento(total, porcentaje) -> Decimal usando descuento() 
2. Escribe la función calcular_cambio(total, pago) -> Decimal | None que devuelva None si el pago no alcanza.
3. Con un while, pide pesos con input() hasta que escribas fin y muestra el total vendido con formato_pesos