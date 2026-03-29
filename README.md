# 🇵🇪 Scraper — Invierte.pe / MEF

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![Selenium](https://img.shields.io/badge/Selenium-4.x-green?logo=selenium)](https://www.selenium.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

Conjunto de **Jupyter Notebooks** para extracción automatizada de datos de inversiones públicas del portal **Invierte.pe / SSI MEF (Perú)**.

Cada notebook es independiente y extrae un tipo de dato a partir de una lista de CUIs (Código Único de Inversión).

---

## 📁 Estructura del repositorio

```
scraper-invierte-mef/
│
├── 01. Extracción Valores COSTO / DEVENGADO / PIM / PIA / FECHAS (SSI MEF)
│   ├── Extracción Valores_SSI Web Scraping_v2.ipynb   ← Notebook principal
│   └── requirements.txt
│
├── 02. Extracción Valores Saldo y Programación Financiera del F12B
│   └── Web Scraping_Extracción Saldo y Programación F12B.ipynb
│
├── 03. Extracción Valores Etapa_F8
│   └── Web Scraping_Extracción Etapa F8.ipynb
│
├── 04. Extracción Valores Montos Programación Anual Financiera del PMI
│   └── Web Scraping_Extracción Montos PMI.ipynb
│
└── 05. Extracción Valores Tipo de Inversión y Unidad Ejecutora (SSI MEF)
    └── Web Scraping_Extracción TIPO DE INVERSIÓN Y UEI del SSI.ipynb
```

---

## 🔍 ¿Qué extrae cada notebook?

| # | Fuente | Datos extraídos |
|---|--------|-----------------|
| 01 | `ofi5.mef.gob.pe/ssi/` | Costo, Devengado, PIM, PIA, Fechas de inicio/fin, Primer devengado, Avance físico |
| 02 | `ofi5.mef.gob.pe/inviertews/Repseguim/ResumF12B` | Programación Financiera Actualizada, Déficit o Saldo |
| 03 | `ofi5.mef.gob.pe/invierte/ejecucion/verFichaEjecucion` | Etapa actual (F8): Aprobación ET, Expediente Técnico, Ejecución física |
| 04 | `ofi5.mef.gob.pe/invierte/pmi/consultapmi` | OPMI, Montos programados 2026–2029 |
| 05 | `ofi5.mef.gob.pe/ssi/` | Tipo de Inversión, Unidad Ejecutora de Inversiones (UEI) |

---

## ✅ Requisitos

- Python 3.10 o superior
- Google Chrome instalado
- Jupyter Notebook / JupyterLab

### Instalar dependencias

```bash
pip install selenium webdriver-manager openpyxl pandas
```

O usando el archivo de requerimientos del módulo 01:

```bash
pip install -r "01.Extracción Valores.../requirements.txt"
```

> **Nota:** `webdriver-manager` descarga automáticamente el ChromeDriver compatible con tu versión de Chrome. No necesitas instalarlo manualmente.

---

## 🚀 Instrucciones de uso

Todos los notebooks siguen el mismo flujo de trabajo:

### 1. Prepara tu archivo de entrada

Crea un archivo Excel llamado **`CUI.xlsx`** con una columna llamada **`CUI`**:

| CUI |
|-----|
| 2507146 |
| 2654533 |
| 2690987 |

Colócalo en la **misma carpeta** que el notebook que vas a ejecutar.

### 2. Abre el notebook en Jupyter

```bash
jupyter notebook
```

### 3. Ejecuta las celdas en orden

Cada notebook está dividido en pasos numerados:

| Paso | Qué hace |
|------|----------|
| **PASO 1** | Instala dependencias (solo la primera vez) |
| **PASO 2** | ⚠️ **Configuración** — único paso que puedes editar |
| **PASO 3** | Importa librerías |
| **PASO 4** | Carga funciones de checkpoint |
| **PASO 5** | Define funciones de extracción |
| **PASO 6** | Carga los CUIs y el navegador |
| **PASO 7** | ▶️ **Ejecuta el scraping** |
| **PASO 8** | Muestra estadísticas del resultado |

> Solo necesitas editar la **celda de Configuración (PASO 2)** para cambiar el nombre del archivo de entrada o salida.

### 4. Resultado

El notebook genera automáticamente un archivo `.xlsx` de resultados en la misma carpeta.

---

## ⚙️ Configuración (PASO 2 de cada notebook)

```python
from pathlib import Path

# Detecta automáticamente la carpeta donde está el notebook
CARPETA_BASE = Path.cwd()

# ← Solo edita estos nombres si tu archivo tiene un nombre diferente
NOMBRE_ARCHIVO_ENTRADA = "CUI.xlsx"
NOMBRE_ARCHIVO_SALIDA  = "resultados.xlsx"

RUTA_ENTRADA = CARPETA_BASE / NOMBRE_ARCHIVO_ENTRADA
RUTA_SALIDA  = CARPETA_BASE / NOMBRE_ARCHIVO_SALIDA
```

Gracias al uso de `Path.cwd()`, el código funciona en **cualquier computadora** sin necesidad de cambiar rutas absolutas.

---

## 💾 Reanudación automática

Si el scraping se interrumpe (por error de red, luz, etc.), simplemente **re-ejecuta las celdas PASO 6 y PASO 7**. El notebook detectará automáticamente los CUIs ya procesados y continuará desde donde se quedó.

```
📥 Progreso encontrado: 320 CUIs ya procesados
⏳ Pendientes: 617
⚡ Listo para procesar 617 CUIs
```

---

## 📊 Ejemplo de output (consola)

```
================================================================
🚀 INICIO: 23:45:04
================================================================

🆕 [1/937] 2507146: ✅ Ejecución física (C) (1.0s)
🆕 [2/937] 2654533: ⚠️ Etapa no encontrada 🔄2... ❌ (2.2s)
🆕 [3/937] 2690987: ✅ Ejecución física (C) (0.8s)
   💾 Guardado
   📊 2✅ 1❌ | 14s | ETA: 22.1min | 12.8 CUI/min
```

---

## 🔧 Opciones avanzadas

En la celda de configuración puedes ajustar:

| Variable | Valor por defecto | Descripción |
|---|---|---|
| `MODO_VISIBLE` | `True` | `False` = Chrome en modo silencioso (sin ventana) |
| `MAX_REINTENTOS` | `2` | Reintentos por CUI fallido |
| `GUARDAR_CADA` | `5` | Frecuencia de guardado automático |
| `TIMEOUT_PAGINA` | `20` | Segundos máx. para cargar una página |
| `TIMEOUT_ELEMENTO` | `10` | Segundos máx. para encontrar un elemento |

---

## ⚠️ Consideraciones

- Este scraper es de **uso personal/académico** para consultar datos públicos del portal Invierte.pe.
- La velocidad promedio es de ~12–13 CUIs/minuto. Para listas grandes, el proceso puede tardar varias horas.
- Si el servidor del MEF está caído o lento, aumenta los valores de `TIMEOUT_PAGINA` y `TIMEOUT_ELEMENTO`.
- Los CUIs con resultado `NO DISPONIBLE` son proyectos que no tienen esa información publicada en el portal.

---

## 🗂️ Archivos de salida

| Notebook | Archivo de salida por defecto | Columnas |
|---|---|---|
| 01 | `resultados_SSI.xlsx` | CUI, Costo, Devengado, PIM, PIA, Fechas, Modalidad, Avance Físico |
| 02 | `resultados_saldo_programacion.xlsx` | CUI, Programación Financiera Actualizada, Déficit o Saldo |
| 03 | `resultados_etapa.xlsx` | CUI, Etapa |
| 04 | `resultados_PMI.xlsx` | CUI, OPMI, Monto año 2026–2029 |
| 05 | `resultados_UEI.xlsx` | CUI, Tipo de Inversión, Unidad Ejecutora de Inversiones (UEI) |

---

## 📄 Licencia

Este proyecto está bajo la licencia [MIT](LICENSE).

---

## 👤 Autor

Desarrollado para análisis de inversiones públicas peruanas usando datos abiertos del portal [Invierte.pe](https://ofi5.mef.gob.pe/ssi/).
