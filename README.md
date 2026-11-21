# Análisis de clientes ConnectaTel

Análisis exploratorio de una empresa de telecomunicaciones (planes Basico y Premium): limpieza de datos, perfil de uso por cliente, segmentación y recomendaciones de negocio.

## Objetivo
Entender cómo usan el servicio los clientes y qué implica para la oferta de planes: calidad de los datos, segmentos de uso y edad, valores extremos y recomendaciones accionables.

## Datos
Tres tablas del curso de TripleTen (no incluidas en este repositorio):
- `plans.csv`: condiciones y precio de los 2 planes (mensajes, minutos, GB incluidos).
- `users_latam.csv`: 4.000 clientes (edad, ciudad, fecha de registro, plan, fecha de baja).
- `usage.csv`: eventos de llamadas y mensajes, del 1 de enero al 30 de junio de 2024.

## Metodología
1. **Carga y exploración** de las tres tablas.
2. **Calidad de datos:** nulos, centinelas (-999 en edad, "?" en ciudad), fechas imposibles e incoherencias entre tablas. Cada decisión de limpieza está documentada en el notebook.
3. **Limpieza** sin eliminar filas: centinelas a nulo, mediana solo con valores válidos.
4. **Perfil por cliente:** agregación de `usage` por `user_id` y unión validada (`validate="one_to_one"`).
5. **Distribuciones y outliers** (boxplots y método IQR).
6. **Segmentación** por uso (Bajo / Medio / Alto) y por edad (Joven / Adulto / Adulto Mayor).
7. **Insight ejecutivo** con recomendaciones.

## Resultados principales
- **Datos:** 55 edades centinela (1,4 %), 565 ciudades no válidas (14,1 %), 40 fechas de registro imposibles (1,0 %) y un 25,2 % de eventos anteriores al registro del cliente, lo que indica que `reg_date` es poco fiable.
- **Uso:** ≈ 74 % de clientes de uso medio, ≈ 19 % de bajo y ≈ 7 % de alto, con distribución casi idéntica en ambos planes.
- **Plan frente a consumo:** el uso mensual medio (≈ 0,9 mensajes y ≈ 3,8 minutos) queda muy por debajo de lo incluido; Premium (25 USD) no consume más que Basico (12 USD).
- **Outliers:** el exceso de minutos (96 clientes, 2,4 %) viene de llamadas más largas, no de más llamadas.
- **Edad:** 50,4 % adultos, 30,6 % adultos mayores y 19,0 % jóvenes, sin diferencias apreciables de uso entre grupos.
- **Recomendaciones:** revisar la propuesta de valor de Premium, estudiar un plan de entrada más ajustado, validar y atender el segmento de llamadas largas, y reforzar la validación de datos en origen.

## Limitaciones
Solo seis meses de datos, sin datos de consumo en GB, `reg_date` no fiable, edades imputadas en un 1,4 % de clientes y conclusiones que suponen un registro de uso completo.

## Cómo ejecutarlo
1. Instala las dependencias: `pip install -r requirements.txt`.
2. Coloca los tres CSV en una carpeta `data/`.
3. En la primera celda cambia `DATA_DIR = Path("/datasets")` (ruta de la plataforma del curso) por `DATA_DIR = Path("data")`.
4. Ejecuta el notebook de arriba abajo (Kernel → Restart & Run All).

## Herramientas
Python, pandas, NumPy, seaborn y matplotlib.
