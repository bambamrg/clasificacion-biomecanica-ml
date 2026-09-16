# Clasificación biomecánica de pacientes ortopédicos

## Descripción del proyecto

Este proyecto desarrolla y compara modelos de machine learning para predecir si
un paciente pertenece a la clase `Normal` o `Abnormal` utilizando mediciones
biomecánicas de la pelvis y la columna vertebral.

El proyecto aborda un flujo completo de trabajo de ciencia de datos:

- Preparación de los datos.
- Análisis exploratorio.
- Entrenamiento de modelos.
- Evaluación de métricas.
- Validación cruzada estratificada.
- Ajuste de hiperparámetros.
- Comparación y selección de modelos.

Este es un proyecto académico y de portfolio. No reemplaza un diagnóstico
médico ni una evaluación realizada por un profesional de la salud.

## Problema de negocio

En un escenario de screening o apoyo a la toma de decisiones, un modelo de
clasificación podría ayudar a identificar pacientes que requieran una
evaluación adicional a partir de sus mediciones biomecánicas.

El objetivo principal es detectar la clase `Abnormal`, manteniendo un equilibrio
razonable entre recall y precision.

Una solución de este tipo podría ser de interés para:

- Analistas de datos.
- Investigadores en biomecánica.
- Equipos de análisis de datos médicos.
- Profesionales interesados en el análisis de riesgo musculoesquelético.

## Dataset

El proyecto utiliza el dataset **Biomechanical Features of Orthopedic
Patients**, obtenido desde Kaggle.

Fuente:

[Biomechanical Features of Orthopedic Patients - Kaggle](https://www.kaggle.com/datasets/uciml/biomechanical-features-of-orthopedic-patients)

El dataset contiene 310 registros y seis variables biomecánicas relacionadas
con la pelvis y la columna lumbar. La variable objetivo clasifica los registros
como `Normal` o `Abnormal`.

Según la información publicada en Kaggle, el dataset se encuentra bajo la
licencia `CC0: Public Domain`.

Las variables predictoras son:

- `pelvic_incidence`
- `pelvic_tilt_numeric`
- `lumbar_lordosis_angle`
- `sacral_slope`
- `pelvic_radius`
- `degree_spondylolisthesis`

La variable objetivo es:

- `class`

### Variables predictoras

- `pelvic_incidence`
- `pelvic_tilt_numeric`
- `lumbar_lordosis_angle`
- `sacral_slope`
- `pelvic_radius`
- `degree_spondylolisthesis`

### Variable objetivo

- `class`: `Normal` o `Abnormal`

## Objetivos del análisis

Los objetivos principales fueron:

1. Comprender las características del dataset.
2. Analizar la relación entre las variables biomecánicas.
3. Entrenar modelos de clasificación.
4. Comparar una regresión logística con árboles de decisión.
5. Evaluar el rendimiento mediante validación cruzada.
6. Ajustar los hiperparámetros del árbol de decisión.
7. Seleccionar el modelo con mejor equilibrio entre las métricas.

## Metodología

El proyecto siguió las siguientes etapas:

1. Carga y revisión de los datos.
2. Análisis de tipos de datos y valores faltantes.
3. Análisis exploratorio de datos.
4. Separación entre variables predictoras y variable objetivo.
5. División de los datos en entrenamiento y prueba.
6. Estandarización de variables para la regresión logística.
7. Entrenamiento de una regresión logística.
8. Entrenamiento de un árbol de decisión.
9. Análisis de importancia de variables.
10. Validación cruzada estratificada.
11. Ajuste de hiperparámetros con `GridSearchCV`.
12. Comparación final de los modelos.

## Modelos utilizados

### Regresión logística

La regresión logística se utilizó como modelo baseline y como candidata a modelo
final.

Debido a que este modelo es sensible a la escala de las variables, se aplicó
estandarización mediante un pipeline de Scikit-learn.

### Árbol de decisión

Se entrenó un árbol de decisión con una profundidad limitada para controlar su
complejidad y reducir el riesgo de sobreajuste.

Una ventaja importante del árbol es su interpretabilidad, ya que sus decisiones
pueden representarse mediante reglas y umbrales.

### Árbol de decisión ajustado

Se utilizó `GridSearchCV` para evaluar diferentes combinaciones de:

- `criterion`
- `max_depth`
- `min_samples_split`
- `min_samples_leaf`

La búsqueda se orientó a optimizar el F1-score de la clase `Abnormal`.

## Métricas de evaluación

Se utilizaron las siguientes métricas:

- **Accuracy**: proporción total de predicciones correctas.
- **Recall**: proporción de pacientes `Abnormal` detectados correctamente.
- **Precision**: proporción de predicciones `Abnormal` que realmente pertenecen
  a esa clase.
- **F1-score**: equilibrio entre precision y recall.
- **Matriz de confusión**: distribución de aciertos y errores por clase.

El recall fue especialmente relevante porque los falsos negativos representan
pacientes `Abnormal` clasificados como `Normal`.

## Resultados

Los modelos se compararon mediante validación cruzada estratificada de cinco
particiones.

| Modelo | Accuracy | Recall `Abnormal` | Precision `Abnormal` | F1-score `Abnormal` |
|---|---:|---:|---:|---:|
| Regresión logística | 0.852 ± 0.034 | 0.890 ± 0.044 | 0.895 ± 0.056 | 0.891 ± 0.024 |
| Árbol de decisión | 0.819 ± 0.033 | 0.871 ± 0.058 | 0.866 ± 0.036 | 0.867 ± 0.026 |
| Árbol ajustado | 0.829 ± 0.026 | 0.905 ± 0.034 | 0.852 ± 0.020 | 0.877 ± 0.020 |

## Selección del modelo

La regresión logística fue seleccionada como modelo principal porque presentó
el mejor rendimiento general en las métricas evaluadas.

Obtuvo:

- El mayor accuracy promedio.
- La mayor precision promedio.
- El mayor F1-score promedio.
- Un recall cercano al del árbol ajustado.
- Un equilibrio más favorable entre detección y falsas alarmas.

El árbol de decisión ajustado alcanzó el mayor recall para la clase `Abnormal`.
Por lo tanto, podría ser considerado si el objetivo prioritario fuera reducir
los falsos negativos, aun aceptando una mayor cantidad de falsos positivos.

El árbol también se conserva como modelo complementario por su facilidad de
interpretación.

## Principales hallazgos

- `degree_spondylolisthesis` fue la variable más importante del árbol de
  decisión.
- La regresión logística obtuvo el mejor equilibrio general.
- El árbol ajustado alcanzó el mayor recall promedio.
- El ajuste de hiperparámetros mejoró el rendimiento del árbol original.
- La selección del modelo depende del costo relativo de los falsos negativos y
  falsos positivos.
- La validación cruzada produjo resultados más conservadores que la evaluación
  basada en una única división train/test.

## Limitaciones

- El dataset es relativamente pequeño.
- Las métricas pueden variar según la partición de los datos.
- No se realizó validación externa sobre otro dataset.
- La evaluación se realizó con un conjunto de datos académico.
- Los resultados pueden no generalizar a otras poblaciones o instituciones.
- Sería conveniente confirmar el rendimiento mediante validación cruzada
  anidada o un conjunto de prueba completamente independiente.
- Los modelos no deben interpretarse como herramientas de diagnóstico clínico.

## Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/bambamrg/clasificacion-biomecanica-ml.git
cd clasificacion-biomecanica-ml
```

### 2. Crear un entorno virtual

En Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Instalar las dependencias

```bash
pip install -r requirements.txt
```

### 4. Descargar el dataset

Descargá el dataset desde:

[Biomechanical Features of Orthopedic Patients - Kaggle](https://www.kaggle.com/datasets/uciml/biomechanical-features-of-orthopedic-patients)

Colocá el archivo CSV dentro de:

```text
data/
```

La estructura esperada será:

```text
data/
├── README.md
└── column_2C_weka.csv
```

El archivo CSV no se incluye en el repositorio y debe descargarse directamente
desde Kaggle.

### 5. Ejecutar el notebook

Abrí el proyecto en VS Code y ejecutá:

```text
notebooks/01_biomechanical_classification_modeling.ipynb
```

## Tecnologías utilizadas

- Python.
- Pandas.
- NumPy.
- Matplotlib.
- Seaborn.
- Scikit-learn.
- Jupyter Notebook.

## Estructura del proyecto

```text
biomechanical-classification-ml/
│
├── data/
│   └── README.md
│
├── notebooks/
│   └── Modelo de clasificacion biomecanica.ipynb
│
├── reports/
│   └── figures/
│
├── Readme.md
├── requirements.txt
└── .gitignore
```

## Autor

Iván Aranda

## Descargo de responsabilidad

Este proyecto fue desarrollado con fines educativos y de portfolio. No
proporciona asesoramiento médico, diagnóstico ni recomendaciones de tratamiento.
