# Machine Learning Aves - Argentina

Este repositorio contiene un pipeline completo de Machine Learning para la predicción de especies de aves en Argentina, basado en datos de avistamientos de la plataforma GBIF (Global Biodiversity Information Facility) y variables bioclimáticas.

## Descripción de los Archivos

### Datasets (CSV)
*   **dataset-2025.csv**: Dataset original con los registros de avistamientos de aves obtenidos de GBIF. Contiene información taxonómica, geográfica y temporal.
*   **dataset-2025-definitivo.csv**: Dataset procesado tras el pipeline de ETL. Incluye las variables bioclimáticas extraídas de los archivos .tif y ha pasado por un proceso de limpieza y filtrado de especies.

### Capas Ambientales (TIF)
Archivos raster que proporcionan las variables explicativas para los modelos:
*   **annual_prec_argentina.tif**: Datos de precipitación anual en Argentina.
*   **elevation_argentina.tif**: Datos de elevación (altitud) del territorio argentino.
*   **mean_temp_argentina.tif**: Datos de temperatura media anual en Argentina.

### Notebooks (Jupyter Notebooks)
El proyecto se divide en dos etapas principales: Extracción, Transformación y Carga (ETL) y Modelado.

#### 1. ETL (ETL-AvistamientosAves-v3.ipynb)
Este notebook realiza la preparación de los datos:
*   Carga de avistamientos y limpieza de registros nulos o inconsistentes.
*   Extracción de valores de precipitación, temperatura y elevación para cada coordenada geográfica.
*   Tratamiento de especies con pocos registros (clases raras).
*   Codificación de variables categóricas (LabelEncoding y OneHotEncoding).

#### 2. Modelado (v2 y v3)
En esta etapa se entrenan y evalúan modelos predictivos utilizando los algoritmos:
*   **Decision Tree (Árboles de Decisión)**
*   **Random Forest (Bosques Aleatorios)**
*   **XGBoost (Extreme Gradient Boosting)**

**Diferencias entre versiones:**
*   **Modelo-AvistamientosAves-v2.ipynb**: Implementa un enfoque estándar donde las especies con menos de un umbral determinado de avistamientos son eliminadas del dataset para evitar el ruido y mejorar la precisión en las especies más frecuentes. Utiliza técnicas de balanceo como SMOTE.
*   **Modelo-AvistamientosAves-v3-TaxonesAgrupados.ipynb**: Introduce una estrategia de **agrupación taxonómica**. En lugar de eliminar las especies poco frecuentes, intenta agruparlas por Género, Familia u Orden, permitiendo que el modelo aprenda patrones de grupos taxonómicos superiores cuando no hay datos suficientes para una especie específica.

## Pipeline General
El flujo de trabajo sigue el esquema:
1.  **ETL**: Procesamiento de datos de GBIF + Integración de variables climáticas (.tif) -> Generación del dataset definitivo.
2.  **Modelado**: Entrenamiento de modelos comparando Decision Tree, Random Forest y XGBoost para predecir la probabilidad de presencia de especies.
