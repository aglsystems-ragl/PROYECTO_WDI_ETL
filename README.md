# ETL + Data Warehouse — World Development Indicators (WDI)

## Integrantes
- **RODRIGO ANDRES GOMEZ LOPEZ** 
- **BRAYAN GONZALES**


---

## 1) Introducción

Este proyecto construye un proceso **ETL completo** usando datos del **Banco Mundial (World Development Indicators - WDI)**.

El objetivo es migrar datos desde un archivo CSV hacia **MySQL**, transformarlos, organizarlos en un **modelo dimensional (esquema estrella)** y luego analizarlos mediante **consultas SQL tipo OLAP** y **visualizaciones**.

Acá no es solo “hacer gráficas”: la idea es estructurar los datos como lo haría un equipo real de **Data Engineering**:

- Cargar datos crudos (RAW / staging)
- Limpiar y transformar a formato analítico (CLEAN)
- Diseñar el **Data Warehouse** (dimensiones + tabla de hechos)
- Validar integridad del modelo (sin llaves nulas)
- Generar análisis y visualizaciones **desde la base de datos**

---

## 2) Requirements Gathering + SDG (ODS)

### Objetivo del proyecto
Construir un pipeline ETL para analizar indicadores globales del Banco Mundial, con énfasis en métricas de salud y desarrollo, dejando el resultado final en un Data Warehouse en esquema estrella.

### Preguntas guía (lo que buscamos responder)
1. ¿Cómo ha cambiado la **esperanza de vida promedio** a nivel global con el tiempo?
2. ¿Qué tan completos son los datos reportados (cobertura / nulos)?
3. ¿Cómo se comporta **Colombia** comparada con el promedio mundial?
4. (Extra) ¿Hay diferencias entre esperanza de vida femenina y masculina?

### Alineación con los ODS
Este análisis se alinea principalmente con el **ODS 3: Salud y Bienestar**, ya que utiliza indicadores como:
- Esperanza de vida al nacer (total / femenina / masculina)
- Fertilidad
- Tasas demográficas relacionadas

Estos indicadores reflejan condiciones de salud, calidad de vida y desarrollo humano.

---

## 3) Evaluación de la fuente de datos (Data Source Evaluation)

### Dataset seleccionado
- **World Development Indicators (WDI)** — Banco Mundial  
- Formato: CSV  
- Archivo usado en el proyecto: `WDI_10k_AGL.csv` (muestra trabajable)

### ¿Por qué este dataset?
- Es **público**, confiable y ampliamente usado.
- Tiene indicadores sociales/económicos desde **1960**.
- Posee estructura útil para ETL (país + indicador + años).
- Tiene valores faltantes (algo normal en datos globales) y permite análisis de calidad.

---

## 4) Arquitectura del proyecto (ETL Architecture)

Flujo general:

1. **Ingesta**: CSV (fuente)
2. **Almacenamiento RAW (staging)**: tabla `wdi_raw` en MySQL
3. **Transformación**: conversión **wide → long** (años a filas: `Year`, `Value`)
4. **Almacenamiento CLEAN**: tabla `wdi_clean` en MySQL
5. **Data Warehouse**: modelo estrella:
   - `dim_country`
   - `dim_indicator`
   - `dim_date`
   - `fact_wdi`
6. **OLAP / SQL**: consultas con JOIN fact + dims
7. **Visualización**: gráficas en Python, alimentadas desde SQL (no desde CSV)

---

## 5) Stack Tecnológico (Technology Stack)

- **Python**: ETL y análisis
- **Pandas**: transformaciones, limpieza y carga
- **SQLAlchemy + PyMySQL**: conexión Python ↔ MySQL
- **MySQL**: base de datos relacional (RAW, CLEAN y DW)
- **Matplotlib**: visualizaciones
- **Jupyter Notebook / VS Code**: ejecución y documentación del proceso
- **Git + GitHub**: versionamiento y repo

---

## 6) Estructura del repositorio (Folder Structure)

> Esta estructura puede variar ligeramente según el repo, pero la idea es mantenerlo ordenado.

```text
.
├── data/
│   ├── raw/
│   │   └── WDI_10k_AGL.csv
│   └── processed/
│       └── (opcional: exports o archivos intermedios)
├── docs/
│   └── imagenes/
│       ├── modelo_relacional.png
│       └── modelo_estrella.png
├── notebooks/
│   └── ETL_WDI.ipynb
├── .gitignore
└── README.md