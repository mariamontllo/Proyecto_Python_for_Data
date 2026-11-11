# Proyecto EDA - Campaña de Marketing Bancario

Análisis exploratorio de datos sobre una campaña de marketing telefónico de un banco portugués

Maria Montlló

---

## Descripción

Este proyecto forma parte del módulo "Python for data" y consiste en realizar un análisis exploratorio de datos (EDA) sobre una campaña de marketing bancario. El objetivo es entender qué factores influyen en que un cliente contrate un depósito a plazo, trabajando con dos datasets distintos que he tenido que combinar, limpiar y analizar.

El análisis está organizado en 6 secciones: introducción, exploración inicial, limpieza de datos, análisis descriptivo, visualizaciones y conclusiones.

---

## Estructura del proyecto

proyecto-eda/
├── data/
│   ├── raw/                          # Datos originales sin modificar
│   │   ├── bank-additional.csv       # Dataset principal (43.000 registros)
│   │   └── customer-details.xlsx     # Datos de clientes (3 hojas: 2012, 2013, 2014)
│   └── processed/                    # Datos transformados y gráficos
│       ├── df_limpio.csv
│       ├── plot_conversion.png
│       ├── plot_edad_educacion.png
│       ├── plot_duracion.png
│       └── plot_ingresos.png
├── notebooks/
│   └── eda_analisis.ipynb            # Notebook principal con todo el análisis
└── README.md

---

## Herramientas utilizadas

- Python 
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Visual Studio Code + Jupyter Notebook

---

## Datasets

**bank-additional.csv** — 43.000 registros de llamadas de la campaña de marketing. Incluye variables sociodemográficas del cliente (edad, trabajo, estado civil, educación), datos de la campaña (número de contactos, duración de la llamada) y variables macroeconómicas (euribor3m, tasa de empleo). La variable objetivo es `y`: si el cliente contrató o no el depósito a plazo.

**customer-details.xlsx** — Información complementaria de los clientes organizada en 3 hojas de trabajo (una por año: 2012, 2013 y 2014). Incluye datos como ingresos anuales, número de hijos, visitas mensuales a la web y fecha de alta como cliente.

---

## Pasos seguidos

1. **Introducción** — Presentación del proyecto, los datasets y el objetivo del análisis.

2. **Exploración inicial** — Cargué el CSV con `pd.read_csv` y el Excel con `pd.ExcelFile`, iterando sobre las 3 hojas y añadiendo una columna `year` antes de unirlas con `pd.concat`. Después hice un LEFT JOIN entre los dos datasets usando el ID del cliente como clave. El resultado fue un dataset combinado de 43.000 filas y 32 columnas.

3. **Limpieza de datos** — Analicé los valores nulos columna por columna y apliqué diferentes estrategias:
   - Variables numéricas (age, euribor3m): imputación con la mediana
   - Variables categóricas (job, education, marital): imputación con 'unknown'
   - Variables binarias (housing, loan): imputación con la moda
   - `cons.price.idx`: eliminada porque quedó completamente vacía tras el merge
   - `date`: eliminadas las 248 filas con valor nulo (0.58% del total)

4. **Análisis descriptivo** — Creé nuevas variables (franja de edad), usé `groupby`, `apply`, `lambda`, `loc` y `pivot_table` para analizar tasas de conversión por segmento. También usé Python base (diccionarios, bucles, funciones propias) para calcular y comparar estadísticas.

5. **Visualizaciones** — Creé 4 gráficos con Matplotlib para ilustrar los hallazgos: distribución de la variable objetivo y conversión por trabajo, distribución de edad y conversión por educación, duración de llamadas por resultado e ingresos por resultado.

6. **Conclusiones** — Resumen de los principales hallazgos y recomendaciones para mejorar la campaña.

---

## Lo que más me costó

**El separador del CSV**
Al cargar el archivo `bank-additional.csv` el resultado era un DataFrame de 43.000 filas pero con una sola columna, lo que no tenía ningún sentido. Después de revisar el archivo entendí que estaba usando `sep=';'` cuando el separador real era una coma. Un error pequeño pero que me hizo perder un buen rato hasta que lo identifiqué.

**Columnas numéricas que se leían como texto**
Cuando intenté calcular la mediana de `euribor3m` y `age` para rellenar los nulos, me daba un error de tipo. El problema era que Pandas había leído esas columnas como texto en lugar de como números. Tuve que usar `pd.to_numeric(df['col'], errors='coerce')` antes de poder hacer cualquier operación estadística sobre ellas. No sabía que esto podía pasar y fue algo nuevo para mí.

**Combinar las 3 hojas del Excel**
El archivo `customer-details.xlsx` tenía los datos repartidos en 3 hojas (una por año: 2012, 2013 y 2014), y al principio no tenía claro cómo unirlas de forma que no perdiera la información del año de cada registro. La solución fue añadir una columna `year` a cada hoja antes de hacer el `pd.concat`, para poder identificar después de qué año venía cada fila.

---

## Uso de IA como apoyo

Durante el proyecto utilicé IA como apoyo en los momentos en los que me quedaba encallada. No para que me diera las soluciones directamente, sino para entender qué estaba fallando y poder seguir avanzando por mi cuenta. Fue especialmente útil cuando los errores no eran de lógica sino de tipo de datos o de cómo Pandas lee ciertos archivos, que no son fáciles de detectar a simple vista. Ver el error explicado en el contexto exacto de lo que estaba haciendo fue en sí mismo un aprendizaje.

---

## Principales hallazgos

**Solo el 11.25% de los clientes contactados contrató el depósito.** Es una tasa de conversión baja, lo que es habitual en campañas de marketing directo donde la mayoría de contactos no convierten.

**El perfil que más contrata son los estudiantes (31.4%) y los jubilados (25.2%)**, muy por encima de la media. Si además filtramos por franja de edad, los clientes mayores de 60 años tienen una tasa de conversión del 39%, la más alta de todo el análisis. Los trabajadores de cuello azul son los que menos convierten (6.9%).

**La duración de la llamada es el factor más diferenciador**: los clientes que contrataron tuvieron llamadas de 9.2 minutos de media, frente a 3.7 minutos los que no contrataron. Cuando un cliente está interesado, la conversación se alarga de forma natural.

**Los ingresos no marcan la diferencia**: los ingresos anuales son prácticamente iguales entre los dos grupos (~93.000), lo que sugiere que no es un factor determinante en la decisión de contratar.

---

## Autora

Maria Montlló  
[@mariamontllo](https://github.com/mariamontllo)
