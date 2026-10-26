# Olist ML Challenge — Semana 12

## Objetivo
Desarrollar un modelo de aprendizaje automático supervisado para estimar qué pedidos de comercio electrónico podrían entregarse después de la fecha prometida, siguiendo la guía del Laboratorio 1 — Reto tercer corte.

## Estructura
- `data/raw/`: datos originales. No se publican en el repositorio.
- `data/processed/`: datos preparados y regenerables.
- `notebooks/S12_ML_Challenge.ipynb`: cuaderno de análisis de la semana 12.
- `src/`: espacio para funciones reutilizables de retos posteriores.
- `reports/`: reporte JSON generado por el cuaderno.
- `.gitignore`: excluye datos y archivos temporales.

## Cómo trabajar
1. Abra `notebooks/S12_ML_Challenge.ipynb` en Google Colab o Jupyter.
2. Introduzca el ID numérico asignado por el profesor (por ejemplo, ID `01` se ingresa como `1`).
3. Ejecute todas las celdas en orden y complete las actividades y respuestas de interpretación.
4. Guarde el cuaderno actualizado en `notebooks/`.
5. Copie el reporte JSON generado por el cuaderno a `reports/`, usando el nombre exigido por la guía, por ejemplo `S12_resultados_ID_01.json`.
6. Registre los avances mediante commits descriptivos.

## Importante
- No suba los CSV originales de Olist ni muestras completas de 35.000 pedidos.
- No invente métricas ni cree manualmente un reporte de resultados: agregue el JSON generado por el cuaderno.
- Mantenga la misma muestra y particiones entre semanas para que los resultados sean comparables.
- `src/` puede permanecer vacío durante S12; la guía indica que no es necesario crear módulos Python externos en este laboratorio.

## Commits sugeridos
- `S12: modelo base`
- `S12: métricas y conclusiones`
