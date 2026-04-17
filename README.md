#  ETL - Análisis de Causas de Mortalidad

##  Descripción

Este proyecto implementa un proceso **ETL (Extract, Transform, Load)** para analizar datos de causas de mortalidad.

El flujo incluye:

* Limpieza de datos
* Estandarización de texto
* Clasificación por categorías clínicas
* Análisis de frecuencia
* Carga en base de datos MySQL

Dataset utilizado: `Causas_de_mortalidad.csv`

---

##  Tecnologías utilizadas

* Python
* pandas
* numpy
* matplotlib
* seaborn
* sqlalchemy
* pymysql
* python-dotenv

---

##  Instalación

```bash
pip install pandas numpy matplotlib seaborn sqlalchemy pymysql python-dotenv
```

---

##  Estructura del proyecto

```
├── data/
│   └── Causas_de_mortalidad.csv
├── .env
├── etl.py
├── etl_errores.log
└── README.md
```

---

##  Variables de entorno

Crear archivo `.env`:

```
DB_USER=0
DB_PASSWORD=0
DB_HOST=0
DB_PORT=0
DB_NAME=tu_db
```

---

##  Proceso ETL

### 1. Extract

Carga del dataset:

```python
df = pd.read_csv('data/Causas_de_mortalidad.csv')
```

---

### 2. Transform

####  Limpieza de datos

* Eliminación de valores nulos
* Conversión de tipos
* Normalización de texto

```python
df['Edad'] = df['Edad'].fillna(round(df['Edad'].mean()))
df['Género'] = df['Género'].replace({'MASCULINO':'M','FEMENINO':'F'})
```

---

#### 🔤Estandarización de texto

Se utilizan expresiones regulares para reducir ruido en las causas de mortalidad:

```python
df['causa_limpia'] = df['Causas de mortalidad'].apply(limpiar_texto)
```

---

####  Clasificación

Agrupación en categorías clínicas:

* cardiovascular
* respiratorio
* trauma
* shock_hipovolemico
* infeccioso
* neurologico
* metabolico
* oncologico
* otros

```python
df['categoria'] = df['causa_limpia'].map(categorias).fillna('otros')
```

---

### 3. Load

Carga a MySQL con manejo eficiente de lotes:

```python
df.to_sql(
    'fact_mortalidad',
    con=engine,
    if_exists='append',
    index=False,
    chunksize=1000,
    method='multi'
)
```

---

##  Manejo de errores

* Validación de DataFrame vacío
* Validación de valores nulos
* Registro de errores en archivo `etl_errores.log`

---

##  Resultados

###  Top 4 causas de mortalidad

```python
df['causas_de_mortalidad'].value_counts().head(4)
```

Ejemplo:

* Infarto agudo de miocardio
* Hemorragia masiva
* Insuficiencia respiratoria
* Infarto agudo del miocardio

---

##  Visualización

```python
top_mortalidad.plot.bar(
    title="Causas de mortalidad",
    ylabel="Cantidad"
)
```




##  Conclusión

Este proyecto demuestra un flujo ETL completo:

* Limpieza de datos reales con ruido
* Transformación semántica
* Persistencia en base de datos
* Generación de insights relevantes

---
