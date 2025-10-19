# Python_EDA
Analisis EDA para Proyecto 4 - unidad Python for data

## Requisitos del proyecto.
A lo largo de este proyecto se cubrirán los siguientes puntos:

● Transformación y limpieza de los datos.  
● Análisis descriptivo de los datos.  
● Visualización de los datos.  
● Informe explicativo del análisis.  

## Herramientas utilizadas.

● Python  
● Pandas  
● Visual Studio Code

## Descripción de datos utilizados

**Contexto:**
Los conjuntos de datos a utilizarse en este proyecto, están relacionados con campañas de marketing directo de una institución bancaria portuguesa. 

**1. Archivo csv - Bank-additional**
Muestra el registro de las llamadas telefónicas relacionadas a las campañas de marketing de la empresa. Es importante recordar, que a menudo, se requiere más de un contacto con el mismo cliente para determinar si el producto (depósito a plazo bancario) sería suscrito o no.
Este dataset consta de: 

**2. Archivo excel - customer-details**
Brinda información sobre las características demográficas y comportamiento de compra de los clientes del banco. Este Excel consta de 3 hojas de trabajo diferentes, en cada una de ellas tenemos los clientes que entraron en el banco en diferentes años. 

## Método de entrega.
● Archivo README.md, que recoja los pasos seguidos durante el proyecto y el análisis.  
● Carpeta de datos llamada **Manejo_de_datos** con los archivos en bruto, asociados a este proyecto, y los datos guardados después de las transformaciones.  
● Carpeta de codigo llamada **Queries_python_eda** con los notebooks o archivos py donde se han realizado todos los pasos pedidos en el proyecto  

## INFORME DE LOS RESULTADOS Y PRINCIPALES PASOS LLEVADOS A CABO 

Total de filas procesadas del dataset final: 43000

**1) Calidad de datos y cambios realizados:**
- Edad (age): imputada por mediana por grupo 'job' y convertida a entero. Mediana global usada: 38.0 años.
- Se reemplazaron valores age==0 por NaN antes de imputar (mejor tratamiento de datos inválidos).
- Columnas eliminadas por no aportar (ej.): 'latitude', 'longitude', 'default'.
- Columnas numéricas convertidas a float (ej.: emp.var.rate, cons.price.idx, cons.conf.idx, euribor3m, nr.employed) y limpieza de separadores decimales.
- Columna 'date' convertida a datetime y usada para agregaciones temporales.

**2) Nuevas columnas creadas (y su objetivo):**
- 'duracion_minutos': duración de la llamada en minutos (con 1 decimal).
- 'grupo_duracion': buckets de duración (0-5, 5-10, ...).
- 'rangos_edad': bucket de edad (menos de 25, 25-50, 50-75, >75).
- 'ultimo_contacto': bucket de pdays (menos de 6 meses, 6-12 meses, ...).
- 'total_hijos', 'rango_ingresos', 'tenure_years', 'visitas_mensuales' en el dataset de clientes.

**3) Resultados agregados / distribuciones relevantes:**
- Media mensual (total_visitas) (serie clientes): 19895.72 (valor medio usado en gráficas).
- Distribución por rango de ingresos (principales grupos):
  50k a 100k (28.7%), 100k a 150k (28.6%), 0 a 50k (25.2%), 150k a 200k (17.4%)

- Resumen (ej. 'rango_ingresos' conteos):
  0 a 50k: 10897
  50k a 100k: 12409
  100k a 150k: 12347

**4) PCA (componentes principales) - variables con mayor carga absoluta:**
  PC1: nr.employed, emp.var.rate, tenure_years
  PC2: duracion_minutos, duration, total_hijos
  PC3: total_hijos, duration, duracion_minutos

**5) Correlaciones ejemplares (post-limpieza):**
 - age vs duracion_minutos: Pearson = 0.016, Spearman = 0.027 (correlación débil).

**6) Unión de datasets:**
 - LEFT JOIN realizado entre Bank_registros (left) y Caracteristicas_clientes (right).
 - Dimensiones resultado (merged): 43000 filas x 34 columnas.

**7) Analisis Avanzado - Correlación de variables y aplicación de Modelo predictivo:**

**Resultados modelo de regresión logística**

Rendimiento del modelo (conjunto de prueba): 
- Tamaño conjunto prueba: 10124 registros
- Accuracy : 0.895
- Precision: 0.588
- Recall   : 0.248
- F1 score :0.349
- ROC AUC  : 0.902

**Variables más influyentes (según los coeficientes del modelo):**
- Efectos positivos (aumentan prob. de conversión):
• duration: coef=1.189, exp(coef)=3.285
• previous: coef=0.204, exp(coef)=1.226
• tenure_years:coef=0.140, exp(coef)=1.151
- Efectos negativos (disminuyen prob. de conversión):
• euribor3m: coef=-0.837, exp(coef)=0.433
• emp.var.rate:coef=-0.396, exp(coef)=0.673

**Interpretación y conclusiones prácticas:** 
- Si 'duration' aparece con coeficiente positivo (habitual): llamadas más largas aumentan la probabilidad de convertir;
priorizar calidad de contacto y seguimiento en llamadas que alcancen cierto
umbral de minutos.
- Si 'previous' tiene coef positivo: clientes contactados previamente tienden a convertir más (o menos si es negativo) — ajustar
estrategia según signo.
- Coeficientes negativos en variables macro (ej.euribor3m, emp.var.rate) sugieren que condiciones económicas más adversas
reducen la conversión.
- Verificar balance de clases: si hay desbalance, las métricas agregadas (accuracy) pueden ser engañosas; priorizar recall/precision
según objetivo del negocio.
- Recomendaciones: aplicar validación cruzada,calibrar probabilidades y probar modelos alternativos (random forest, xgboost) y
técnicas de balanceo si es necesario.
