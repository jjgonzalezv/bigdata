# Predicción de incumplimiento de pago (American Express): procesamiento eficiente de grandes volúmenes de datos

**Estudiante:** Cristian Camilo Cuellar Moya · **Asignatura:** Big Data · **Docente:** Jose John Fredy Gonzales Veloza

## Pregunta de investigación
¿Qué combinación de formato (CSV o Parquet) y motor (Polars o DuckDB, con evaluación perezosa y streaming) agrega por cliente con menos tiempo y memoria en una máquina de 8 GB de RAM?

## Datos
Kaggle, *American Express - Default Prediction*: `train_data.csv` (15,27 GiB, 5.531.451 filas, 190 columnas) y `train_labels.csv` (458.913 clientes). Los datos no están en el repositorio por su tamaño (límite de 100 MB de GitHub); el notebook los descarga con `kagglehub`.

## Resultado principal
| Tarea 3 (agregación completa) | Tiempo | RAM pico |
|---|---|---|
| CSV + Polars | 78,01 s | 448 MB |
| CSV + DuckDB | 80,46 s | 673 MB |
| Parquet + Polars | 0,81 s | 473 MB |
| Parquet + DuckDB | 1,04 s | 440 MB |

Con Parquet las tareas corren entre ~77 y ~930 veces más rápido que con CSV; entre motores la diferencia es menor que 2 veces. Convertir a Parquet redujo el tamaño de 15,27 a 5,76 GiB (−62,3 %). **Conclusión: Parquet + Polars.**

## Técnicas de clase aplicadas
- Evaluación perezosa y plan de ejecución
- Streaming (procesamiento por lotes)
- Almacenamiento columnar Parquet: metadatos, grupos de filas, pushdown de proyección y filtro
- Estadísticas desde datos intermedios (n, Σx, Σx²)

Spark se evaluó en modo local solo como comparación y se justifica por qué no se usó; MapReduce solo se menciona.

## Contenido
- `proyecto_tercer_corte.ipynb`: notebook completo (secciones 1 a 11)
- `comparacion_formato_motor.png`, `comparacion_spark.png`: gráficas de resultados

## Cómo reproducir
1. Entorno de 8 GB de RAM (GitHub Codespaces) con `polars`, `duckdb`, `pyarrow`, `kagglehub`, `matplotlib`.
2. Aceptar las reglas de la competencia en Kaggle y crear un token propio.
3. Ejecutar el notebook en orden; pedirá el token por `getpass` (no se guarda en el repositorio).

## Limitaciones
Las mediciones en CSV son una sola corrida; las tareas usan solo 4 columnas; el filtro por fecha no salta grupos de filas porque el archivo está ordenado por cliente.
