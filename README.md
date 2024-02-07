
# Proyecto de Machine Learning DevOps 🤖📊

Este repositorio contiene el código y los recursos relacionados con el proyecto de Machine Learning DevOps realizados para el primer proyecto individual del bootcamp SoyHendy de la carrera de DataScience. Aquí encontrarás información sobre el flujo de trabajo de forma resumida para la puesta en marcha del proyecto.

## Datos Iniciales 📊

![Datos Iniciales](assets\datos_iniciales.PNG)

## ETL 🛠️

### Transformaciones generales de ETL inicial 🔄

- Importación correcta de datos de origen en formato JSON.
- Desanidación de datos.
- Eliminación de columnas irrelevantes, manejo de valores nulos y duplicados.
- Normalización de tipos de datos.
- Creación de la columna "sentiment_analysis" aplicando NLP al dataframe de Reviews.
- Realización de transformaciones para la correcta utilización de los datos por parte de las funciones.
- Guardado del dataset procesado en formato Parquet.

## Funciones de Consulta 📝

Se desarrollaron varias funciones de consulta para analizar y extraer información del conjunto de datos. A continuación se detallan algunas de estas funciones junto con ejemplos de su retorno:

### `Developer_Reviews_Analysis(desarrolladora: str)` 📈

Esta función devuelve un diccionario con el nombre del desarrollador como clave y una lista con la cantidad total de registros de reseñas de usuarios que se encuentren categorizados con un análisis de sentimiento como valor positivo o negativo.

**Ejemplo de retorno:**

```json
{
  "Valve": {
    "Negative": 182,
    "Positive": 278
  }
}
```

### `Best_Developer_Year(año: int)` 🥇

Devuelve el top 3 de desarrolladores con juegos MÁS recomendados por usuarios para el año dado (reviews.recommend = True y comentarios positivos).

**Ejemplo de retorno:**

```json
[
  {"Puesto 1": "desarrollador_X"},
  {"Puesto 2": "desarrollador_Y"},
  {"Puesto 3": "desarrollador_Z"}
]
```

### `UserForGenre(genero: str)` 🎮

Devuelve el usuario que acumula más horas jugadas para el género dado y una lista de la acumulación de horas jugadas por año de lanzamiento.

**Ejemplo de retorno:**

```json
{
  "Usuario con más horas jugadas para Género X": "us213ndjss09sdf",
  "Horas jugadas": [
    {"Año": 2013, "Horas": 203},
    {"Año": 2012, "Horas": 100},
    {"Año": 2011, "Horas": 23}
  ]
}
```

### `UserData(user_id: str)` 💰👤

Esta función toma como argumento un user_id y devuelve cantidad de dinero gastado por el usuario, el porcentaje de recomendación en base a reviews.recommend y cantidad de items.

**Ejemplo de retorno:**

```json
{
  "Usuario X": "us213ndjss09sdf",
  "Dinero gastado": "200 USD",
  "Porcentaje de recomendación": "20%",
  "Cantidad de items": 5
}
```

### `Developer(desarrollador: str)` 📅

Devuelve cantidad de items y porcentaje de contenido Free por año según empresa desarrolladora.

**Ejemplo de uso:**

```json
{
 'Año': [2008],
 'Cantidad de Items': [1],
 'Contenido Free': [100.0]
}
```

# Exploratory Data Analysis (EDA) 📊

## Objetivos Generales 🎯
- Detectar outliers y aplicar técnicas de tratamiento de datos para asegurar la representatividad del modelo de Machine Learning.
- Identificar las características más relevantes del conjunto de datos para implementar un modelo de recomendación de Machine Learning.
- Profundizar en la comprensión de las relaciones generales entre las variables del conjunto de datos.

# Machine Learning 🤖

## Funciones Desarrolladas para el Sistema de Recomendación 🎮

### Sistema de Recomendación Item-Item 🛒🛍️:

#### `recomendacion_juego(item_id):`

Ingresando el ID de un producto, se espera recibir una lista con 5 juegos recomendados similares al ingresado.

Para esta función, se utilizó `TfidfVectorizer` de `sklearn.feature_extraction.text` sobre la columna `genres`. Luego, se calcularon los features (en `genres`) para cada `item_id`. Posteriormente, se calcularon las similitudes entre todos los documentos, se crearon las predicciones para todos los datos y se exportó el dataframe para ser consumido por la función final en la API. Esto se realizó debido a las limitaciones de procesamiento que ofrece `render` (plataforma donde se realizó el despliegue).

### Sistema de Recomendación User-Item 🎮👤:

#### `recomendacion_usuario(user_id):`

Ingresando el ID de un usuario, se espera recibir una lista con 5 juegos recomendados para dicho usuario.

Para esta función, se utilizó `item_id` y `user_id` con respecto al `playtime_forever`. Se normalizaron los datos de los tiempos de juego en un rango de 0 a 1 y luego se empleó `GridSearchCV` con el algoritmo SVD para realizar una búsqueda en cuadrícula con validación cruzada con el algoritmo SVD y medidas ["rmse", "fcp"]. Finalmente, se seleccionó el modelo con el mejor puntaje y se realizaron las predicciones pertinentes. Nuevamente, se exportaron las predicciones para ser consumidas directamente por la API.

# API deploy 🚀

## Descripción General ℹ️

La API está desarrollada utilizando FastAPI, una librería de Python para construir APIs web rápidas y seguras. Proporciona endpoints para consultar información sobre desarrolladores de juegos, datos de usuarios, estadísticas por género, análisis de reseñas y los dos sistemas de recomendación de juegos de los que hablamos en el apartado anterior.

## Endpoints Disponibles 📡

- `/api/desarrollador/{desarrollador}`: Devuelve información sobre un desarrollador específico.
- `/api/datos_usuario/{user_id}`: Proporciona datos de usuario basados en su ID.
- `/api/usuario_por_genero/{genero}`: Ofrece estadísticas sobre usuarios por género.
- `/api/mejor_desarrollador/{anio}`: Devuelve el mejor desarrollador para un año dado.
- `/api/reviews_por_desarrolladora/{desarrolladora}`: Proporciona análisis de reseñas para un desarrollador.
- `/api/recomendacion_item/{item_id}`: Sistema de recomendación item-item para juegos.
- `/api/recomendacion_usuario/{user_id}`: Sistema de recomendación user-item para usuarios.

## Uso 🕹️

Para utilizar la API, simplemente sigue este enlace: [API de Machine Learning del Conjunto de Datos de Steam](https://steam-data-project.onrender.com/docs).

¡Esperamos que disfrutes explorando los datos y utilizando los modelos de Machine Learning proporcionados por la API! 🚀