# Predicción de Calidad del Agua (WQI) con PySpark y Keras

**Pontificia Universidad Javeriana**
**Procesamiento de Alto Volumen de Datos**

**Autor:** Mauricio Raba
**Profesor:** John Corredor
**Fecha:** 28/04/2026

---

## Descripción del Proyecto

Este proyecto implementa un pipeline de análisis de datos para evaluar la calidad del agua en ríos de la India, utilizando **PySpark** para el procesamiento distribuido y **Keras** para la construcción de un modelo predictivo del índice **WQI (Water Quality Index)**.

Se sigue una metodología basada en **CRISP-DM**, cubriendo:

* Preprocesamiento de datos
* Análisis exploratorio (EDA)
* Cálculo del WQI
* Visualización geográfica
* Modelado con redes neuronales

---

## Contenido del Repositorio

* `Clean_ML_Water.ipynb` - Notebook principal
* `waterquality.csv` - Dataset
* `Indian_States`- Archivos Shapefile para visualización geográfica
---

## 🧪 Dataset

El dataset contiene mediciones fisicoquímicas y bacteriológicas de ríos en distintos estados de la India (fuente: RiverIndia).

### Variables principales

| Parámetro           | Descripción          | Unidad    | Rango Óptimo |
| ------------------- | -------------------- | --------- | ------------ |
| TEMP                | Temperatura del agua | °C        | 15 – 25      |
| DO                  | Oxígeno disuelto     | mg/L      | ≥ 6.0        |
| pH                  | Nivel de acidez      | —         | 7.0 – 8.5    |
| CONDUCTIVITY        | Conductividad        | µS/cm     | 0 – 75       |
| BOD                 | Demanda bioquímica   | mg/L      | < 3.0        |
| NITRATE_N_NITRITE_N | Nitratos/Nitritos    | mg/L      | < 20         |
| FECAL_COLIFORM      | Coliformes fecales   | UFC/100mL | < 5          |

La columna `TOTAL_COLIFORM` fue eliminada por no aportar valor predictivo.

---

## Preprocesamiento de Datos

* Verificación de valores nulos (no se encontraron)
* Conversión de tipos `String → Float`
* Eliminación de variables irrelevantes
* Filtrado de registros válidos

---

## Análisis Exploratorio (EDA)

Se realizó análisis estadístico descriptivo y visualizaciones clave:

* Relación entre **DO y pH**
* Relación entre **BOD y nitratos**
* Relación entre **conductividad y coliformes**

Esto permitió identificar patrones de contaminación y condiciones del agua.

---

## Cálculo del WQI

El índice WQI se calcula como:

[
WQI = \sum (qr_i \times w_i)
]

Donde:

* `qr`: rango de calidad por parámetro
* `w`: peso del parámetro

### Pesos principales

* DO → 0.281
* Coliformes fecales → 0.281
* Conductividad → 0.234
* pH → 0.165

DO y coliformes representan **más del 56% del peso total**, siendo los factores más críticos.

---

## Clasificación del Agua

| Categoría  | Rango WQI | Interpretación   |
| ---------- | --------- | ---------------- |
| Excelente  | 0 – 25    | Agua potable     |
| Buena      | 25 – 50   | Calidad moderada |
| Baja       | 50 – 75   | Agua dura        |
| Muy Baja   | 75 – 100  | Agua muy dura    |
| Inadecuada | > 100     | Agua residual    |

---

## Visualización Geográfica

Se generó un mapa de la India utilizando **GeoPandas**, integrando:

* Datos del WQI por estado
* Shapefiles geográficos
* Normalización de nombres
* Imputación de valores faltantes

Esto permite identificar regiones con mayor problemática de calidad del agua.

---

##  Modelo de Machine Learning

Se implementó una red neuronal con **Keras (MLP)**:

### Arquitectura

* 3 capas ocultas de 350 neuronas (ReLU)
* Capa de salida lineal
* ~248,000 parámetros

### Configuración

* Optimizador: Adam
* Loss: MSE
* Épocas: 200
* Split: 80/20

---

## Evaluación del Modelo

Aunque metodológicamente la evaluación corresponde a una fase posterior, en este proyecto se analiza a partir del comportamiento del entrenamiento:

* La función de pérdida muestra **convergencia rápida**
* El modelo aprende correctamente la relación entre variables
* Posible **sobreajuste** debido al tamaño del dataset

Importante:
El WQI es una combinación lineal de las variables, por lo que el modelo está aprendiendo una relación **determinista**.

Esto implica que:

* Una red neuronal es **innecesariamente compleja**
* Una **regresión lineal** sería suficiente y más interpretable

---

## Limitaciones

* Dataset pequeño (~30 registros)
* Variables discretas (valores limitados)
* No se aplicó validación cruzada
* No se comparó con modelos clásicos (MLlib)

---

## Conclusiones

* PySpark permite un procesamiento eficiente de datos de calidad hídrica
* El WQI es una métrica interpretable y útil
* La visualización geográfica aporta valor analítico
* El modelo neuronal funciona, pero está sobredimensionado
* Problema idealmente resoluble con regresión lineal

---

## Trabajo Futuro

* Implementar regresión lineal (MLlib)
* Incluir métricas: R², MAE, RMSE
* Aplicar regularización (Dropout, L2)
* Ampliar el dataset
* Usar validación cruzada

---

## Referencias

1. Sutadian et al. (2016) – Water Quality Index
2. RiverIndia Dataset
3. Apache Spark MLlib Docs
4. François Chollet – Deep Learning with Python
5. Material de clase – PUJ

---

## Notas Finales

Este proyecto tiene fines académicos.
El WQI calculado no debe interpretarse como indicador real de potabilidad, sino como ejercicio de modelamiento.

---
