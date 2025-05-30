# Aprendizaje-no-supervisado

## Descripción del Proyecto

Este proyecto tiene como objetivo analizar un conjunto de datos de clientes de un banco (`Churn_Modelling.csv`) para identificar patrones y segmentos de clientes utilizando técnicas de aprendizaje no supervisado, específicamente el algoritmo K-means. El análisis se centra en comprender las características de los diferentes grupos de clientes y cómo se relacionan con la tasa de abandono (Churn).

Se exploran tres enfoques principales para el clustering:

1. **Dataset Original Estandarizado**: Aplicando K-means directamente sobre las variables numéricas estandarizadas.
2. **Reducción de Dimensionalidad (PCA)**: Utilizando las dos primeras Componentes Principales obtenidas de PCA.
3. **Selección de Características**: Agrupando sobre un subconjunto de variables seleccionadas basándose en su importancia y correlación.

Se comparan las métricas de clustering (Inercia y Silhouette Score) entre estos tres enfoques para determinar cuál ofrece los agrupamientos más coherentes. Finalmente, se interpreta el cluster seleccionado para identificar los factores asociados con el abandono de clientes.

## Objetivos

- **Preparación de Datos**: Cargar y estandarizar variables numéricas clave.
- **Reducción de Dimensionalidad**: Aplicar PCA para simplificar el conjunto de datos y visualizarlo.
- **Selección de Características**: Identificar y seleccionar variables relevantes usando técnicas de correlación y Random Forest.
- **Clustering K-means**: Aplicar K-means en las tres versiones del dataset (original estandarizado, PCA, selección de características).
- **Evaluación de Clustering**: Utilizar el Método del Codo y Silhouette Score para determinar el número óptimo de clusters y comparar la calidad de los agrupamientos entre los diferentes enfoques.
- **Análisis de Clusters**: Interpretar las características de los clusters resultantes, especialmente en relación con la tasa de abandono de clientes.

## Dataset Utilizado

El dataset `Churn_Modelling.csv` contiene información de 10,000 clientes de un banco. Las variables numéricas clave utilizadas para el clustering son:

- `CreditScore`: Puntuación de crédito del cliente.
- `Age`: Edad del cliente.
- `Tenure`: Años que el cliente ha permanecido en el banco.
- `Balance`: Saldo en la cuenta bancaria.
- `NumOfProducts`: Número de productos bancarios.
- `HasCrCard`: Si tiene tarjeta de crédito (1=Sí, 0=No).
- `IsActiveMember`: Si es miembro activo (1=Sí, 0=No).
- `EstimatedSalary`: Salario estimado.
- `Exited`: Si el cliente abandonó el banco (1=Sí, 0=No). (Variable objetivo para análisis, no para clustering directo).

## Herramientas y Librerías

- Python 3.x
- Pandas: Para manipulación y análisis de datos.
- NumPy: Para operaciones numéricas.
- Scikit-learn: Para estandarización (`StandardScaler`), PCA (`PCA`), K-means (`KMeans`) y métricas de evaluación (`silhouette_score`), y Random Forest (`RandomForestClassifier`).
- Matplotlib y Seaborn: Para visualización de datos y gráficos.

## Pasos Realizados

1. **Carga y Exploración de Datos**: Se cargó el dataset y se realizó una inspección inicial de sus características.
2. **Selección y Estandarización**: Se seleccionaron 8 variables numéricas y se estandarizaron utilizando `StandardScaler`.
3. **Análisis PCA**:
   - Se aplicó PCA al dataset estandarizado.
   - Se generó un Scree Plot para visualizar la varianza explicada y acumulada.
   - Se redujo el dataset a las 2 primeras componentes principales para visualización.
   - Se creó un diagrama de dispersión de las dos componentes principales, coloreado por `Exited`.
   - Se calcularon los loadings para interpretar la contribución de las variables originales.
4. **Selección de Características**:
   - Se generó una matriz de correlación.
   - Se entrenó un `RandomForestClassifier` para obtener la importancia de cada característica.
   - Se decidió eliminar la variable `HasCrCard` debido a su baja importancia, simplificando el modelo sin pérdida significativa de información.
5. **Clustering con K-means**:
   - Se aplicó K-means (K de 2 a 6 clusters) a las tres versiones del dataset: Original Estandarizado, PCA (2 Componentes), y Selección de Características (sin `HasCrCard` y estandarizado).
   - Se calcularon y graficaron el Método del Codo (Inercia) y el Silhouette Score para cada ejecución para evaluar la calidad del clustering.
6. **Análisis Detallado y Comparación**:
   - Se seleccionó K=4 para el dataset con Selección de Características para un análisis más profundo.
   - Se visualizaron los clusters en el espacio de las dos primeras componentes principales de PCA.
   - Se generó un resumen de las características promedio por cluster y se calculó la tasa de abandono por cluster.
   - Se compararon gráficamente las curvas de Inercia y Silhouette Score para los tres enfoques.

## Resultados y Conclusiones Clave

### Evaluación de Enfoques de Clustering

- **Dataset Original Estandarizado**: Logró un Silhouette Score máximo de 0.508 en K=4, indicando clusters razonablemente bien definidos.
- **PCA (2 Componentes)**: Los Silhouette Scores fueron consistentemente más bajos (por debajo de 0.40), sugiriendo que la reducción a solo dos componentes podría haber perdido demasiada información crucial para un clustering óptimo.
- **Selección de Características**: Los resultados de inercia y Silhouette Score fueron prácticamente idénticos al dataset Original Estandarizado. Esto es una conclusión positiva, ya que se simplificó el modelo (eliminando `HasCrCard`) sin comprometer la calidad del agrupamiento.

**Conclusión Principal**: La selección de características (eliminando `HasCrCard`) demostró ser el enfoque más eficiente, ya que mantuvo la calidad de clustering del dataset original estandarizado con un modelo ligeramente más simple y, por ende, potencialmente más interpretable.

### Análisis de Clusters (Basado en el dataset de Selección de Características con K=4)

- **Cluster 0 (Tasa de Abandono: 24.9%)**: Este cluster tiene una alta tasa de abandono. Lo componen clientes de mediana edad, con altos saldos y sueldos estimados, pero con pocos productos contratados y un nivel moderado de actividad.
- **Clusters 1 y 3 (Tasa de Abandono: 13.2% y 15.2%)**: Ambos clusters presentan saldos bajos, pero se diferencian en el salario estimado (cluster 3 con alto salario, cluster 1 con bajo salario). Su tasa de abandono es significativamente menor que la del cluster 0.
- **Cluster 2 (Tasa de Abandono: 22.8%)**: Similar al cluster 0 en cuanto a altos saldos y sueldos estimados, con una tasa de abandono ligeramente menor.

**Recomendaciones**: El banco debería enfocarse en el Cluster 0 para desarrollar estrategias de retención. Es crucial investigar por qué clientes con buenos recursos financieros y salarios deciden abandonar, especialmente si no tienen muchos productos bancarios con la institución.
