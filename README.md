# Titulo del Proyecto
Analisis EDA de las campañas de Marketing de una institución Bancaria.

## Descripción del Proyecto
Este proyecto realiza un análisis exploratorio y predictivo de las campañas de marketing de una institución financiera. El objetivo principal del trabajo es comprender mejor los patrones de comportamiento de los clientes, las variables que influyen en la suscripción de productos financieros y extraer conclusiones basadas en datos.

## Descripción de datos utilizados

**Contexto:**
Los conjuntos de datos a utilizarse en este proyecto, están relacionados con campañas de marketing directo de una institución bancaria portuguesa. 

**1. Archivo csv - Bank-additional**
Muestra el registro de las llamadas telefónicas relacionadas a las campañas de marketing de la empresa. Es importante recordar, que a menudo, se requiere más de un contacto con el mismo cliente para determinar si el producto (depósito a plazo bancario) sería suscrito o no.
Este dataset consta de: 

**2. Archivo excel - customer-details**
Brinda información sobre las características demográficas y comportamiento de compra de los clientes del banco. Este Excel consta de 3 hojas de trabajo diferentes, en cada una de ellas tenemos los clientes que entraron en el banco en diferentes años. 

## Requisitos del proyecto.
A lo largo de este proyecto se cubrirán los siguientes puntos:

● Transformación y limpieza de los datos.  
● Análisis descriptivo de los datos.  
● Visualización de los datos.  
● Informe explicativo del análisis.  

## Estructura del Proyecto

🗂️ Carpetas específicas en Github
├── Raw data/ # Datos crudos y datos limpios
├── Notebooks/ # Notebooks de Jupyter con el análisis
├── Informe/ # Archivo explicativo con resultados y gráficos 
├── README.md # Descripción del proyecto


## Método de entrega.
● Archivo README.md, que recoja los pasos seguidos durante el proyecto y el análisis.  
● Carpeta de datos llamada **Manejo_de_datos** dentro de Raw Data con los archivos en bruto asociados a este proyecto, y los datos guardados después de las transformaciones.  
● Carpeta de codigo llamada **Queries_python_eda** dentro de Notebooks con los notebooks o archivos py donde se han realizado todos los pasos pedidos en el proyecto
● Archivo PDF llamado **Informe de resultados - Python Proyecto EDA** con los pasos y resultados del proyecto.

## Herramientas y librerias utilizadas.

● Python
- pandas
- numpy
- matplotlib
- seaborn
● Visual Studio Code
● Microsoft Excel


## RESULTADOS Y PRINCIPALES CONCLUSIONES 

Total de filas procesadas del dataset final tras unir los dos archivos en bruto: 43000

**Transformación y limpieza de los datos.**

**1) Calidad de datos y cambios realizados:**
- Edad (age): imputada por mediana por grupo 'job' y convertida a entero. Mediana global usada: 38.0 años.
- Se reemplazaron valores age==0 por NaN antes de imputar (mejor tratamiento de datos inválidos).
- Columnas eliminadas por no aportar (ej.): 'latitude', 'longitude', 'default'.
- Columnas numéricas convertidas a float (ej.: emp.var.rate, cons.price.idx, cons.conf.idx, euribor3m, nr.employed) y limpieza de separadores decimales.
- Columna 'date' convertida a datetime y usada para agregaciones temporales.

**2) Nuevas columnas creadas:**
- 'duracion_minutos': duración de la llamada en minutos (con 1 decimal).
- 'grupo_duracion': buckets de duración (0-5, 5-10, ...).
- 'rangos_edad': bucket de edad (menos de 25, 25-50, 50-75, >75).
- 'ultimo_contacto': bucket de pdays (menos de 6 meses, 6-12 meses, ...).
- 'total_hijos', 'rango_ingresos', 'tenure_years', 'visitas_mensuales' en el dataset de clientes.

**Análisis descriptivo de los datos mediante Visualización de los mismos**.  

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

**Analisis Avanzados - Correlación de variables y aplicación de Modelo predictivo:**

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

**Conclusiones y recomendaciones**

Este análisis exploratorio nos permitió comprender mejor la estructura del conjunto de datos y los factores que pueden influir en la suscripción de productos bancarios. Se detectó que variables como la duración de la llamada, el nivel educativo y el estado laboral tienen una influencia significativa en la decisión del cliente.

*Conclusiones prácticas:*
•	Si 'duration' aparece con coeficiente positivo, que es lo más habitual, las llamadas más largas aumentan la probabilidad de convertir. Se sugiere priorizar calidad de contacto y seguimiento en llamadas que alcancen cierto umbral de minutos.
•	Si 'previous' tiene coeficiente positivo, los clientes contactados previamente tienden a convertir más (o menos si es negativo). Se sugiere ajustar estrategia según el coeficiente resultante.
•	Coeficientes negativos en variables macro como el euribor3m y emp.var.rate sugieren que condiciones económicas más adversas reducen la conversión.
•	Verificar balance de clases: si hay desbalance, las métricas agregadas como accuracy pueden ser engañosas. Se debe priorizar recall/precisión según la estrategia del banco.

*Recomendaciones:*

•	Priorizar campañas dirigidas a segmentos con mayor propensión a suscribirse según las características identificadas.
•	Continuar monitoreando las variables macroeconómicas (como euribor3m) que muestran correlaciones con las decisiones de los clientes.
•	Mejorar la calidad del registro de datos para reducir la presencia de valores faltantes o inconsistentes.
•	Recomendaciones de técnicas avanzadas:  aplicar validación cruzada, calibrar probabilidades y probar modelos alternativos (random forest, xgboost) y técnicas de balanceo si es necesario.

