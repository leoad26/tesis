# Segmentación de Clientes y Chatbot NLP — Clínica Dra. Pía Noroña Medicina Estética

Sistema de segmentación de clientes mediante *Machine Learning* no supervisado y un chatbot basado en Procesamiento de Lenguaje Natural (NLP), orientado a optimizar las estrategias de marketing y la atención al cliente de la Clínica Dra. Pía Noroña Medicina Estética.

> **Proyecto de Titulación** — Maestría en Inteligencia Artificial Aplicada
> Universidad de las Américas (UDLA), Quito, Ecuador

---

## Descripción del proyecto

El sistema combina dos componentes principales:

1. **Modelo de segmentación de clientes**, que agrupa a los pacientes en segmentos diferenciados mediante algoritmos de *clustering* no supervisado (K-Means, DBSCAN, Gaussian Mixture Models).
2. **Chatbot conversacional** basado en NLP (spaCy + BETO), que responde consultas frecuentes sobre los servicios de la clínica y recomienda tratamientos personalizados.

Ambos componentes se integran para generar recomendaciones automáticas y promociones dirigidas según el perfil de cada cliente. El proyecto se estructura bajo la metodología **CRISP-DM** e incorpora prácticas de **MLOps** (Docker, MLflow, CI/CD) para garantizar la trazabilidad y reproducibilidad de los resultados.

---

## Objetivo general

Desarrollar un sistema de segmentación de clientes y recomendación de servicios mediante algoritmos de *clustering* no supervisado y procesamiento de lenguaje natural, para optimizar las estrategias de marketing y atención al cliente en la clínica.

## Objetivos específicos

| # | Objetivo | Meta cuantificable |
|---|----------|--------------------|
| 1 | Análisis exploratorio (EDA) de los datos históricos de 97 pacientes con Python y Pandas | Identificar ≥ 3 patrones de comportamiento (frecuencia, tipo de servicio, rango de gasto) |
| 2 | Diseñar el modelo de segmentación con ML no supervisado (K-Means, DBSCAN o GMM) | ≥ 3 segmentos diferenciados; índice de silueta ≥ 0,5 |
| 3 | Desarrollar el chatbot NLP (Python, spaCy, *transformers*) | ≥ 90 % de los flujos conversacionales implementados |
| 4 | Integrar segmentación + chatbot para recomendaciones y venta cruzada | Cobertura ≥ 85 % en pruebas de integración |

---

## Estructura del repositorio

```
.
├── data/                  # Datos en sus distintos estados (NO se versionan: LOPDP)
│   ├── raw/               # Datos originales sin modificar (97 registros)
│   ├── processed/         # Datos transformados, listos para modelado
│   └── external/          # Fuentes de datos de terceros
├── notebooks/             # Jupyter notebooks (numerados por orden de ejecución)
│   ├── exploratory/       # Análisis exploratorio (EDA)
│   └── reports/           # Notebooks de reportes y resultados
├── src/                   # Código fuente modular y reutilizable
│   ├── data/              # Carga y preprocesamiento de datos
│   ├── features/          # Ingeniería de variables (feature engineering, RFM)
│   ├── models/            # Algoritmos de clustering y evaluación
│   ├── chatbot/           # Componente NLP (spaCy + BETO)
│   └── visualization/     # Gráficos y visualizaciones
├── models/                # Modelos entrenados (NO se versionan los pesados)
│   ├── trained/           # Modelos serializados (.pkl)
│   └── checkpoints/       # Puntos de control
├── docs/                  # Documentación técnica
│   ├── api/               # Documentación de API
│   └── guides/            # Guías de usuario
├── tests/                 # Pruebas unitarias y de integración
├── config/                # Archivos de configuración (config.yaml)
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Requisitos previos

- Python 3.10 o superior
- pip y virtualenv (o Conda)
- Docker (opcional, para reproducibilidad total del entorno)
- Git

---

## Instalación

```bash
# 1. Clonar el repositorio
git clone https://github.com/<tu-usuario>/<nombre-del-repo>.git
cd <nombre-del-repo>

# 2. Crear y activar un entorno virtual
python -m venv venv
source venv/bin/activate        # En Windows: venv\Scripts\activate

# 3. Instalar las dependencias
pip install -r requirements.txt

# 4. Descargar el modelo de spaCy en español (para el chatbot)
python -m spacy download es_core_news_lg
```

---

## Uso

El flujo completo se ejecuta por etapas, desde los datos crudos hasta los resultados finales:

```bash
# 1. Preprocesamiento y limpieza de datos
python src/data/preprocess.py

# 2. Ingeniería de variables (RFM)
python src/features/build_features.py

# 3. Entrenamiento del modelo de segmentación
python src/models/train_clustering.py

# 4. Evaluación de los clusters (Silhouette, Davies-Bouldin)
python src/models/evaluate.py

# 5. Levantar el chatbot
python src/chatbot/run_bot.py
```

> **Nota:** Todos los parámetros (número de clusters, semillas, rutas) se gestionan desde `config/config.yaml`, no se escriben directamente en el código.

---

## Reproducibilidad

Para garantizar resultados consistentes entre ejecuciones:

- **Semillas aleatorias fijas** (`SEED = 42`) en todos los procesos estocásticos.
- **Versiones exactas** de las librerías especificadas en `requirements.txt`.
- **Docker** para encapsular el entorno completo (sistema operativo, dependencias).
- **Scripts idempotentes** y parametrizables vía `config.yaml`.
- **MLflow** para el registro de experimentos (parámetros, métricas y artefactos).

---

## Protección de datos (LOPDP)

> ⚠️ **Importante:** Este proyecto trabaja con datos reales de pacientes (nombre, teléfono, servicio, monto, fecha).

- **Los datos de pacientes NUNCA se suben al repositorio**, ni siquiera en un repositorio privado, en cumplimiento de la **Ley Orgánica de Protección de Datos Personales (LOPDP)** del Ecuador.
- Las carpetas `data/raw`, `data/processed` y `data/external` están excluidas mediante `.gitignore`.
- El versionado de datos se gestiona por separado (DVC con almacenamiento remoto).
- Para el desarrollo y la documentación se utilizan datos anonimizados.

---

## Metodología

El proyecto sigue la metodología **CRISP-DM** (*Cross-Industry Standard Process for Data Mining*):

1. Comprensión del negocio
2. Comprensión de los datos
3. Preparación de los datos
4. Modelado
5. Evaluación
6. Despliegue

---

## Stack tecnológico

| Área | Herramientas |
|------|--------------|
| Análisis y ML | Python, Pandas, Scikit-learn |
| Clustering | K-Means, DBSCAN, Gaussian Mixture Models |
| NLP / Chatbot | spaCy, BETO (BERT en español) |
| MLOps | Docker, MLflow, GitHub Actions (CI/CD) |
| Evaluación | Índice de silueta, índice de Davies-Bouldin |

---

## Equipo

| Integrante | Rol |
|------------|-----|
| Esteban Cherrez | Gestión de procesos, arquitectura y MLOps |
| Ariel Altamirano | Programación y desarrollo de ML |
| Karen Jurado | Marketing y diseño conversacional (NLP) |

**Tutor de tesis:** asesoría y aprobación
**Cliente:** Clínica Dra. Pía Noroña Medicina Estética (proveedor de datos)

---

## Licencia

Proyecto académico desarrollado con fines de titulación. Uso restringido conforme a los acuerdos con la clínica y la normativa de la UDLA.
