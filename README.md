# 🌦️ SENAMHI Scraper

> Herramienta de extracción automática de datos hidrometeorológicos del portal [SENAMHI](https://www.senamhi.gob.pe/) — Servicio Nacional de Meteorología e Hidrología del Perú.

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)
![Selenium](https://img.shields.io/badge/Selenium-4.x-green?logo=selenium&logoColor=white)
![Tkinter](https://img.shields.io/badge/UI-Tkinter-lightgrey)
![License](https://img.shields.io/badge/Licencia-MIT-yellow)

---

## 📋 Descripción

SENAMHI Scraper es una aplicación de escritorio con interfaz gráfica (GUI) que automatiza la descarga de datos de estaciones meteorológicas e hidrológicas del portal oficial de SENAMHI. Permite seleccionar un departamento, recorre todas las estaciones disponibles en el mapa interactivo y guarda los datos mensuales de cada estación en archivos `.csv` organizados por carpetas.

### Características principales

- Interfaz gráfica moderna con tema institucional (azul/blanco)
- Selección de departamento desde un menú desplegable (24 departamentos disponibles)
- Extracción automática de todos los meses disponibles por estación
- Fallback inteligente: intenta descarga directa CSV → extracción de tabla → iframe interno
- Barras de progreso en tiempo real (total y por estación)
- Registro de actividad con timestamps y niveles de severidad
- Organización automática de archivos por `Departamento / Estación / YYYYMM.csv`

---

## 🗂️ Estructura de salida

```
senamhi_datos/
└── Puno/
    ├── DESAGUADERO/
    │   ├── 202301.csv
    │   ├── 202302.csv
    │   └── ...
    ├── JULIACA/
    │   └── ...
    └── ...
```

---

## ⚙️ Requisitos

### Sistema operativo
- Windows 10 / 11 (probado)
- La ruta de Chrome se detecta automáticamente en `C:\Program Files\Google\Chrome\`

### Software base

| Requisito | Versión mínima | Notas |
|-----------|---------------|-------|
| Python | 3.9 o superior | [python.org](https://www.python.org/downloads/) |
| Google Chrome | Cualquier versión reciente | Debe estar instalado |
| ChromeDriver | Debe coincidir con tu versión de Chrome | Ver sección de instalación |

---

## 🚀 Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/senamhi-scraper.git
cd senamhi-scraper
```

### 2. Crear entorno virtual (recomendado)

```bash
python -m venv .venv
```

Activar el entorno:

```bash
# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# Windows (CMD)
.venv\Scripts\activate.bat
```

### 3. Instalar dependencias Python

```bash
pip install -r requirements.txt
```

Si no tienes el archivo `requirements.txt`, instala manualmente:

```bash
pip install selenium
```

> **Nota:** `tkinter` viene incluido con Python en Windows. No necesita instalación adicional.

### 4. Instalar ChromeDriver

ChromeDriver debe coincidir exactamente con la versión de tu Google Chrome.

**Opción A — Automática (recomendada):**

```bash
pip install webdriver-manager
```

Luego en el script reemplaza la línea de conexión del driver si deseas usarlo directamente (no necesario para el modo debug que usa este scraper).

**Opción B — Manual:**

1. Abre Chrome y ve a `chrome://settings/help` para ver tu versión
2. Descarga el ChromeDriver correspondiente desde [chromedriver.chromium.org](https://chromedriver.chromium.org/downloads) o [googlechromelabs.github.io/chrome-for-testing](https://googlechromelabs.github.io/chrome-for-testing/)
3. Extrae `chromedriver.exe` y colócalo en una carpeta incluida en el `PATH` del sistema, por ejemplo `C:\Windows\System32\`, o en la misma carpeta del proyecto

**Verificar instalación:**

```bash
chromedriver --version
```

---

## 📦 Archivo `requirements.txt`

```
selenium>=4.0.0
```

Crea este archivo en la raíz del proyecto con ese contenido, o genera uno automáticamente si usas un entorno virtual:

```bash
pip freeze > requirements.txt
```

---

## ▶️ Ejecución

```bash
python scraping_pro.py
```

O si usas el entorno virtual:

```bash
.venv\Scripts\python.exe scraping_v12_pro.py
```

---

## 🖥️ Guía de uso paso a paso

### Paso 1 — Seleccionar departamento

En el menú desplegable **"Departamento"** elige la región de la que quieres extraer datos.

### Paso 2 — Abrir Chrome en modo debug

Haz clic en el botón **"⚡ Abrir Chrome"**.

Esto lanzará Chrome con depuración remota habilitada y abrirá automáticamente la página de estaciones de SENAMHI del departamento seleccionado.

> **Espera** a que el mapa cargue completamente y los pines de las estaciones sean visibles antes de continuar.

### Paso 3 — Iniciar el scraping

Haz clic en **"▶ Iniciar Scraping"**.

El sistema comenzará a:
1. Detectar todas las estaciones en el mapa
2. Hacer clic en cada pin para abrir su popup
3. Navegar a la pestaña de datos tabulares
4. Descargar o extraer los datos de cada mes disponible
5. Guardar cada mes como un archivo `.csv` independiente

### Paso 4 — Ver los archivos generados

Haz clic en **"📂 Ver Carpeta"** para abrir el explorador de archivos en la carpeta de salida.

---

## 📁 Ubicación de los datos

Los archivos se guardan en:

```
<directorio_donde_ejecutas_el_script>\senamhi_datos\
```

Ejemplo: si ejecutas desde `D:\Mineria de datos\Scrapping\`, los datos estarán en:

```
D:\Mineria de datos\Scrapping\senamhi_datos\
```

---

## 🔍 Descripción del log

El panel de registro usa colores para indicar el tipo de mensaje:

| Color | Significado |
|-------|-------------|
| **Verde** | Operación exitosa (archivo guardado, estación completada) |
| **Rojo** | Error (fallo de descarga, error crítico) |
| **Ámbar** | Advertencia (sin popup, sin botón CSV, solo cabecera) |
| **Azul** | Información general (meses disponibles, página activa) |
| **Gris** | Detalle de diagnóstico (botones detectados, resumen parcial) |

---

## ⚠️ Solución de problemas

### `chromedriver` no encontrado

Asegúrate de que `chromedriver.exe` esté en el `PATH`. Puedes comprobarlo con:

```bash
where chromedriver
```

### Chrome no abre (`Chrome no encontrado en rutas predefinidas`)

El script busca Chrome en:
- `C:\Program Files\Google\Chrome\Application\chrome.exe`
- `C:\Program Files (x86)\Google\Chrome\Application\chrome.exe`

Si tu instalación está en otra ruta, edita la lista `CHROME_EXE_PATHS` al inicio del archivo:

```python
CHROME_EXE_PATHS = [
    r"C:\tu\ruta\personalizada\chrome.exe",
]
```

### `selenium.common.exceptions.WebDriverException: Cannot connect to Chrome`

Asegúrate de haber hecho clic en **"⚡ Abrir Chrome"** antes de iniciar el scraping. El puerto de debug (`9222`) debe estar activo.

### Los datos se guardan con pocas columnas o solo `SIN_DATOS`

Algunos meses no tienen datos disponibles en el servidor de SENAMHI. El scraper lo registra como advertencia y continúa con el siguiente mes.

### `_tkinter.TclError: invalid command name`

Verifica que estás usando **Python 3.9 o superior**. Puedes comprobarlo con:

```bash
python --version
```

---

## 🏗️ Estructura del proyecto

```
senamhi-scraper/
├── scraping_v12_pro.py   # Script principal
├── requirements.txt      # Dependencias Python
├── README.md             # Este archivo
└── senamhi_datos/        # Carpeta de salida (se crea automáticamente)
```

---

## 📌 Notas técnicas

- El scraper se conecta a Chrome mediante el protocolo **Chrome DevTools Protocol (CDP)** a través del puerto `9222`. Esto permite controlar una sesión real del navegador sin levantar una instancia nueva.
- Los datos se extraen directamente del DOM (tabla HTML) cuando la descarga directa de CSV no está disponible.
- La carpeta de descarga de Chrome se redirige dinámicamente por cada estación usando el comando CDP `Page.setDownloadBehavior`.

---

## 📄 Licencia

MIT License — libre para uso académico, investigación y proyectos personales.

---

## 👤 Autor

Desarrollado para el curso de **Minería de Datos** como herramienta de recopilación de datos hidrometeorológicos del Perú.
