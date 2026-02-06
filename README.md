# 🏛️ Suite de Scrapers MEF - Sistema de Inversión Pública Perú

Conjunto de herramientas para extraer datos presupuestales del Ministerio de Economía y Finanzas (MEF) del Perú. 

Si trabajas con proyectos de inversión pública y necesitas consultar cientos de CUIs, estos scripts te van a ahorrar días de trabajo manual.

---

## 📁 ¿Qué incluye?

Son **5 scrapers especializados**, cada uno extrae información diferente del sistema del MEF:

| Script | Datos que extrae | Portal |
|--------|-----------------|--------|
| **01. Costo y Devengado** | Costo total, PIA, PIM, devengados 2026, fechas | [SSI MEF](https://ofi5.mef.gob.pe/ssi/) |
| **02. Saldo y Programación** | Programación financiera, déficit/saldo | [Rep. Seguimiento](https://ofi5.mef.gob.pe/inviertews/) |
| **03. Etapa** | Etapa del proyecto (ejecución, formulación, etc.) | [Ficha Ejecución](https://ofi5.mef.gob.pe/invierte/) |
| **04. PMI y Montos Anuales** | OPMI, montos proyectados 2026-2029 | [Consulta PMI](https://ofi5.mef.gob.pe/invierte/pmi/) |
| **05. Tipo y UEI** | Tipo de inversión, unidad ejecutora | [SSI MEF](https://ofi5.mef.gob.pe/ssi/) |

---

## 🚀 Instalación y Uso

### Requisitos previos

- Python 3.8 o superior
- Google Chrome instalado
- Conexión a internet

### 1. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 2. Preparar tu archivo de entrada

Todos los scripts esperan un archivo Excel con una columna `CUI`:

| CUI |
|-----|
| 2123456 |
| 2234567 |
| 2345678 |

### 3. Configurar el script que necesites

Cada script tiene estas variables al inicio que debes ajustar:

```python
RUTA_ENTRADA = "input/cuis.xlsx"      # Tu archivo con CUIs
RUTA_SALIDA = "output/resultados.xlsx" # Donde se guardarán los resultados
MODO_VISIBLE = True                    # Ver navegador (False = oculto)
```

### 4. Ejecutar

```bash
# Ejemplo: extraer costos y devengados
python 01_costo_devengado.py
```

---

## 📊 Ejemplos de Uso

### Caso 1: Análisis presupuestal completo

Si necesitas **todos los datos** de una cartera de proyectos:

1. Ejecuta el **Script 01** → obtienes costos, PIA, PIM, devengados
2. Ejecuta el **Script 05** → obtienes tipo y UEI
3. Ejecuta el **Script 03** → obtienes la etapa
4. Une los Excel resultantes en uno solo

### Caso 2: Seguimiento PMI

Si solo necesitas datos de la **Programación Multianual**:

1. Ejecuta el **Script 04** → obtienes OPMI y montos 2026-2029

### Caso 3: Verificar saldos

Si necesitas verificar **déficit o saldo**:

1. Ejecuta el **Script 02** → obtienes programación y saldo

---

## 💡 Características útiles de todos los scripts

✅ **Sistema de checkpoints** - Si se interrumpe, continúa donde se quedó  
✅ **Guardado progresivo** - Guarda cada 5 CUIs para no perder datos  
✅ **Reintentos automáticos** - Vuelve a intentar CUIs que fallaron  
✅ **Logs en tiempo real** - Ves el progreso mientras trabaja  
✅ **Manejo de errores** - CUIs no disponibles se marcan claramente  

---

## 📋 Detalle de cada scraper

### 01. Extracción de Costos y Devengados

**Datos extraídos:**
- Costo Inversión Total
- PIA 2026
- PIM 2026
- Devengado Acumulado
- Devengado 2026
- Fechas de inicio, fin y primer devengado

**Uso típico:** Análisis de ejecución presupuestal, seguimiento de avance financiero

---

### 02. Extracción de Saldo y Programación

**Datos extraídos:**
- Programación Financiera Actualizada
- Déficit o Saldo

**Uso típico:** Verificar disponibilidad presupuestal, identificar déficits

---

### 03. Extracción de Etapa

**Datos extraídos:**
- Etapa del proyecto (EJECUCIÓN, FORMULACIÓN, etc.)

**Uso típico:** Clasificar proyectos por estado, filtrar por etapa

---

### 04. Extracción PMI y Montos Anuales

**Datos extraídos:**
- OPMI (Órgano Programático del Ministerio)
- Monto año 2026
- Monto año 2027
- Monto año 2028
- Monto año 2029

**Uso típico:** Proyecciones multianuales, planificación presupuestal

**⚠️ Nota:** No todos los CUIs están en el PMI. Los que no existen se marcan como "NO DISPONIBLE"

---

### 05. Extracción de Tipo y UEI

**Datos extraídos:**
- Tipo de Inversión
- Unidad Ejecutora de Inversiones (UEI)

**Uso típico:** Clasificación de proyectos, análisis por entidad ejecutora

---

## ⚙️ Configuración avanzada

Todos los scripts tienen estas opciones configurables:

```python
MAX_REINTENTOS = 2          # Intentos por CUI fallido
MODO_VISIBLE = True         # Ver navegador mientras trabaja
GUARDAR_CADA = 5            # Guardar progreso cada N CUIs
TIMEOUT_PAGINA = 20         # Segundos max para cargar página
TIMEOUT_ELEMENTO = 10       # Segundos max para encontrar elementos
```

---

## 🎯 ¿Para quién es esto?

- **Oficinas de Planeamiento** - Seguimiento de carteras de inversión
- **Analistas de Presupuesto** - Análisis de ejecución y proyecciones
- **Gobiernos Regionales** - Monitoreo de proyectos locales
- **Entidades Fiscalizadoras** - Auditoría y control
- **Investigadores** - Estudios sobre inversión pública
- **Consultores** - Evaluación de proyectos

---

## 📊 Ejemplo de salida en consola

```
📂 Cargando CUIs...
✅ 150 CUIs cargados
📥 Progreso encontrado: 50 CUIs ya procesados
📋 Nuevos: 80 | Reintentos: 20 | Total pendiente: 100

============================================================
🚀 INICIO: 14:30:25
============================================================

[1/100] 2123456: ✅ (2.3s)
[2/100] 2234567: ✅ (1.8s)
[3/100] 2345678: ❌ (3.5s)
[4/100] 2456789: ✅ (2.0s)
[5/100] 2567890: ✅ (2.2s)
   💾 Guardado
   
   📊 4✅ 1❌ | 12s | ETA: 3.2min | 25 CUI/min
```

---

## 🛠️ Tecnologías

- **Python 3.8+** - Lenguaje de programación
- **Selenium** - Automatización del navegador
- **Pandas** - Procesamiento de datos
- **openpyxl** - Manejo de archivos Excel

---

## ⚠️ Consideraciones importantes

**Uso responsable:**
- Estos scripts consultan información pública del MEF
- Respeta los tiempos de carga (no modificar timeouts a la baja)
- Evita consultas excesivas en horarios pico
- Úsalos para análisis legítimo de datos públicos

**Limitaciones:**
- Algunos CUIs pueden no tener datos disponibles
- El script depende de la estructura actual de los portales web
- Si el MEF cambia el diseño de sus páginas, hay que actualizar el código
- Necesitas conexión a internet estable

---

## 🔧 Solución de problemas comunes

**"No se encontró columna CUI"**
- Verifica que tu Excel tenga una columna llamada exactamente `CUI` (mayúsculas)

**"Timeout al cargar página"**
- Tu conexión puede ser lenta, aumenta `TIMEOUT_PAGINA` a 30 o 40

**"Muchos CUIs con NO DISPONIBLE"**
- Normal en el Script 04 (PMI), no todos los CUIs están en ese sistema
- Para otros scripts, puede ser problema temporal del portal MEF

**"Error de ChromeDriver"**
- El script descarga ChromeDriver automáticamente
- Si falla, verifica que Chrome esté actualizado

---

## 📁 Estructura recomendada del proyecto

```
scrapers-mef/
├── 01_costo_devengado.py
├── 02_saldo_programacion.py
├── 03_etapa.py
├── 04_pmi_montos.py
├── 05_tipo_uei.py
├── requirements.txt
├── README.md
├── input/
│   └── cuis.xlsx
└── output/
    ├── resultados_01.xlsx
    ├── resultados_02.xlsx
    ├── resultados_03.xlsx
    ├── resultados_04.xlsx
    └── resultados_05.xlsx
```

---

## 🤝 Contribuciones

Si encuentras bugs o tienes ideas para mejorar:
1. Abre un issue describiendo el problema
2. O mejor aún, envía un pull request con la solución

---

## 📄 Licencia

MIT License - Úsalo libremente.

---

## 📞 Recursos útiles

- [Portal SSI MEF](https://ofi5.mef.gob.pe/ssi/)
- [Portal Invierte.pe](https://ofi5.mef.gob.pe/invierte/)
- [Consulta PMI](https://ofi5.mef.gob.pe/invierte/pmi/)
- [Documentación Selenium](https://selenium-python.readthedocs.io/)

---

**Nota:** Este es un proyecto independiente, no está afiliado oficialmente con el MEF.

Si te fue útil, dale una ⭐ en GitHub
