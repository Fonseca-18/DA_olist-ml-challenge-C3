# 🛒 Olist ML Challenge — S12 y S14

**Reto de Machine Learning | Semanas 12 a 14**

Repositorio académico dedicado al desarrollo del reto de *Machine Learning* utilizando datos del ecosistema de comercio electrónico de Olist. El proyecto organiza los notebooks de análisis y los reportes generados durante el proceso. El problema es **predecir si un pedido llegará después de la fecha estimada de entrega** (`is_late`).

---

## 🎯 Objetivos del proyecto

- **S12:** construir una muestra reproducible de 35.000 pedidos, definir `is_late` y entrenar un modelo base de regresión logística.
- **S14:** aplicar PCA (≥ 90 % de varianza), comparar cuatro regresiones logísticas (con y sin PCA, distintos valores de `C`), calcular el costo empresarial de los errores y comparar `valor_flete` entre entrenamiento y validación con la prueba Kolmogorov-Smirnov.
- Interpretar los resultados y documentar las conclusiones.

## 👨‍💻 Información del estudiante

| Campo | Información |
|---|---|
| **ID del estudiante** | 13 (semilla `1013`) |
| **Actividad** | Machine Learning Challenge |
| **Semanas** | 12 y 14 |
| **Muestra** | 35.000 pedidos · 24.500 entrenamiento / 5.250 validación / 5.250 prueba |
| **Huella de la muestra** | `7c40e95d5d35` |

## 📁 Estructura del repositorio

```text
DA_olist-ml-challenge-C3/
├── data/
│   ├── raw/          # Datos originales (no se suben a GitHub)
│   └── processed/    # Datos procesados (no se suben a GitHub)
├── notebooks/
│   ├── S12_ML_Challenge.ipynb      # Semana 12: modelo base
│   └── S14_ML_Challenge.ipynb      # Semana 14: PCA, 4 modelos, costos y prueba KS
├── src/              # Código reutilizable
├── reports/
│   ├── S12_resultados_ID_13.json   # Resultados de la semana 12
│   ├── resultados_experimentos.csv # Semana 14: métricas y costos de los 4 modelos
│   ├── resumen_analisis.md         # Semana 14: resumen e interpretación
│   └── figures/                    # Gráficas generadas por el notebook S14
├── requirements.txt
├── .gitignore
└── README.md
```

## ⚙️ Herramientas utilizadas

- **Python:** procesamiento y análisis de datos.
- **Jupyter Notebook / Google Colab:** ejecución de actividades y experimentos.
- **Pandas y NumPy:** manipulación de datos.
- **Scikit-learn:** preprocesamiento, PCA, regresión logística y métricas.
- **SciPy:** prueba Kolmogorov-Smirnov (`scipy.stats.ks_2samp`).
- **Matplotlib:** gráficas.
- **Git y GitHub:** control de versiones y documentación del proyecto.

## 🚀 Cómo ejecutar el proyecto

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/Fonseca-18/DA_olist-ml-challenge-C3.git
   cd DA_olist-ml-challenge-C3
   ```

2. Instalar las dependencias (se recomienda un entorno virtual):

   ```bash
   pip install -r requirements.txt
   ```

3. Abrir `notebooks/S14_ML_Challenge.ipynb` en Jupyter Notebook o Google Colab y ejecutar **todas las celdas, de principio a fin** (Kernel → Restart & Run All). Se requiere conexión a internet: el notebook descarga los datos públicos de Olist desde una versión fijada por commit y **no** guarda los datos en el repositorio.
4. El notebook comprueba que la muestra coincide con la de la semana 12 (misma semilla, mismos tamaños y misma huella) y genera automáticamente los reportes de `reports/`.

> `S14_ML_Challenge.ipynb` es autosuficiente: repite la preparación de datos de la semana 12 y no depende de variables de otra sesión.

## 📊 Resultados

| Archivo | Contenido |
|---|---|
| `reports/S12_resultados_ID_13.json` | Resultados del modelo base (semana 12) |
| `reports/resultados_experimentos.csv` | Accuracy, Precision, Recall, F1, Average Precision, matriz de confusión y costo de los modelos A, B, C y D |
| `reports/resumen_analisis.md` | Resumen de PCA, comparación de modelos, costos, prueba KS y conclusiones |
| `reports/figures/` | Varianza explicada (PCA), matrices de confusión, costos y ECDF de `valor_flete` |

Los costos empresariales (falso positivo = R$ 8, falso negativo = R$ 35) son **supuestos académicos** y no cifras reales de Olist.

## 📌 Consideraciones

- Mantener la estructura de carpetas del proyecto.
- No subir datos originales, archivos privados ni conjuntos de datos completos.
- La partición de **prueba** permanece reservada: no se usa para entrenar ni para elegir modelos.
- Imputación, escalado y PCA se ajustan únicamente con el conjunto de entrenamiento.
- Documentar los cambios y avances mediante commits descriptivos.
- Verificar que el notebook se ejecute correctamente antes de entregar el proyecto.

---

**Proyecto académico · Machine Learning · S12 y S14 · ID 13**
