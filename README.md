# Estimación de rentabilidad anual para vivienda turística en Madrid

## Descripción del proyecto

Este proyecto desarrolla un modelo de **Machine Learning** para estimar la **rentabilidad anual de viviendas turísticas en Madrid**.

El objetivo es predecir la variable:

`estimated_revenue_l365d`

que representa el **ingreso estimado generado por una propiedad en los últimos 365 días**.

El proyecto sigue un flujo completo de **Data Science**:

- Exploratory Data Analysis (EDA)
- Preprocesamiento de datos
- Feature Engineering
- Modelado y evaluación
- Optimización de hiperparámetros
- Persistencia del modelo

El objetivo final es identificar **qué variables influyen más en los ingresos de un alojamiento turístico** y construir un modelo capaz de generar **predicciones fiables**.

---

# Dataset

El dataset se construyó a partir de la **unificación de tres subdatasets**, que posteriormente fueron limpiados y procesados.

Las variables incluyen información sobre:

- características del alojamiento
- actividad y volumen de reseñas
- disponibilidad
- restricciones de estancia
- características del anfitrión

La variable objetivo utilizada fue:

`estimated_revenue_l365d`

Esta variable presenta **alta asimetría y presencia de outliers**, por lo que se aplicó una transformación logarítmica:

```python
revenue_log = log1p(revenue)
```

Esto permite reducir el impacto de valores extremos y mejorar el comportamiento de los modelos.

---

# Flujo del proyecto

## Exploratory Data Analysis (EDA)

Durante el análisis exploratorio se evaluaron:

- distribución del target
- análisis de variables numéricas
- análisis de variables categóricas
- correlación con la variable objetivo
- detección de duplicados
- tratamiento de valores nulos
- identificación de outliers

Se detectó que el target presenta una **distribución fuertemente sesgada a la derecha**, con valores extremos elevados.

Las variables con mayor relación con el revenue fueron:

- `estimated_occupancy_l365d`
- `number_of_reviews`
- `reviews_per_month`
- `number_of_reviews_ltm`

Estas variables reflejan **el nivel de actividad y ocupación del alojamiento**.

---

# Preprocesamiento de datos

Durante esta fase se realizaron varias transformaciones.

### Tratamiento de duplicados

Los duplicados se identificaron utilizando el **id del alojamiento**.

### Tratamiento de valores nulos

Se aplicaron distintas estrategias:

- eliminación de columnas con alto porcentaje de nulos
- imputación con mediana
- imputación con moda
- imputación con valores específicos
- creación de categoría `"Unknown"`

### Tratamiento de outliers

Se analizaron los percentiles extremos **1% y 99%**, ajustando valores extremos en algunas variables.

---

# Feature Engineering

Se transformaron variables categóricas en numéricas para permitir el entrenamiento de modelos.

Se utilizaron técnicas como:

- One Hot Encoding
- Codificación binaria
- Mapeo ordinal
- Feature encoding para variables de alta cardinalidad

Para evitar **data leakage**, algunas transformaciones se aplicaron únicamente sobre el conjunto de entrenamiento.

---

# Modelado

Se probaron diferentes algoritmos de regresión:

- Ridge Regression (baseline)
- Random Forest
- XGBoost

Todos los modelos se evaluaron usando:

- **K-Fold Cross Validation (k = 5)**
- `shuffle = True`
- `random_state = 42`

Esto garantiza comparaciones justas entre modelos.

---

# Métrica de evaluación

La métrica principal utilizada fue:

**RMSE (Root Mean Squared Error)**

calculado sobre:

`revenue_log`

Para interpretar resultados en euros se aplica:

```python
revenue = expm1(revenue_log)
```

---

# Selección del modelo

El modelo con mejor rendimiento fue:

**XGBoost**

Este modelo se utilizó para el entrenamiento final.

---

# Optimización de hiperparámetros

Se utilizó:

`RandomizedSearchCV`

en lugar de GridSearch por su mayor eficiencia computacional.

---

# Evaluación final

El modelo final se evaluó sobre un **conjunto de test independiente**, que no se utilizó durante el entrenamiento.

Esto permite estimar el rendimiento del modelo en datos no vistos.

---

# Persistencia del modelo

El modelo final se guardó utilizando:

`joblib`

Esto permite reutilizar el modelo sin necesidad de reentrenarlo.

---

# Resultados

El modelo captura una **parte significativa de la variabilidad del revenue** generado por los alojamientos turísticos.

Sin embargo, el RMSE puede verse afectado por la presencia de **valores extremos**, ya que la distribución del revenue tiene una cola pesada.

---

# Limitaciones

- los datos representan una fotografía temporal del mercado
- factores externos pueden afectar los ingresos futuros:
  - regulación
  - turismo
  - economía local

---

# Tecnologías utilizadas

Python

Principales librerías:

- pandas
- numpy
- scikit-learn
- xgboost
- matplotlib
- seaborn
- joblib

---

# Estructura del repositorio

```
ML_GRUPO_1/

├── main.ipynb
├── modelo_optimizado.pkl
├── X_test.csv
├── y_test.csv
├── ML_ppt.pptx
├── Presentación ML.pdf
└── README.md
```

---

# Autores

- Nazareth Montero
- Javier Pascual
- Román Díaz
- Sara Ruiz