# Resumen de análisis - Semana 14 (Olist ML Challenge, ID 13)

Generado automáticamente por `notebooks/S14_ML_Challenge.ipynb` con los resultados reales de su ejecución.

## 1. Datos y reproducibilidad

- Muestra: 35,000 pedidos (una fila por pedido), semilla 1013, huella `7c40e95d5d35`.
- Particiones: entrenamiento 24,500 / validación 5,250 / prueba 5,250 (la prueba permanece reservada y no se usó).
- Variable objetivo `is_late` (1 = entrega posterior a la fecha estimada); tasa de tardíos en la muestra: 6.77%.
- Preprocesamiento, PCA y escalado ajustados únicamente con entrenamiento. Sin variables posteriores a la entrega.
- Entorno: Python 3.12.3, pandas 2.2.3, numpy 2.5.4, scikit-learn 1.6.1, scipy 1.18.1.

**Nota de reproducibilidad.** El modelo A repite la configuración de la semana 12. En el JSON de la semana 12: Accuracy 0.6571, Recall 0.6348, F1 0.2007, costo R$ 17,910. En esta ejecución: Accuracy 0.6573, Recall 0.6348, F1 0.2008, costo R$ 17,902. Las pequeñas diferencias (del orden de una predicción) pueden deberse a versiones distintas de las librerías numéricas; la muestra y las particiones son idénticas (misma huella).

## 2. Actividad 1 - PCA

Las 24 variables numéricas se resumen en **15 componentes** para conservar al menos el 90% de la varianza (acumulada real: 90.46%). La primera componente explica 18.8% y las tres primeras 36.1%; es decir, la información está repartida entre muchas componentes y no se concentra en una o dos. Reducir de 24 a 15 variables numéricas descarta 9.5% de la variación.

Figura: `reports/figures/pca_varianza_explicada.png`.

## 3. Actividad 2 - Cuatro regresiones logísticas (validación, umbral 0.50, `class_weight='balanced'`)

| modelo | transformacion | C | accuracy | precision | recall | f1 | average_precision |
|---|---|---|---|---|---|---|---|
| A | Sin PCA | 1.0 | 0.6573 | 0.1193 | 0.6348 | 0.2008 | 0.1462 |
| B | PCA 90 % | 1.0 | 0.6419 | 0.1100 | 0.6039 | 0.1861 | 0.1230 |
| C | PCA 90 % | 0.1 | 0.6400 | 0.1099 | 0.6067 | 0.1860 | 0.1236 |
| D | PCA 90 % | 10.0 | 0.6396 | 0.1090 | 0.6011 | 0.1845 | 0.1226 |

## 4. Actividad 3 - Costos empresariales

Costo = 8 x FP + 35 x FN. **Los costos son supuestos académicos; no son cifras reales de Olist.**

| modelo | TN | FP | FN | TP | costo_fp_brl | costo_fn_brl | costo_total_brl |
|---|---|---|---|---|---|---|---|
| A | 3,225 | 1,669 | 130 | 226 | 13,352 | 4,550 | 17,902 |
| B | 3,155 | 1,739 | 141 | 215 | 13,912 | 4,935 | 18,847 |
| C | 3,144 | 1,750 | 140 | 216 | 14,000 | 4,900 | 18,900 |
| D | 3,144 | 1,750 | 142 | 214 | 14,000 | 4,970 | 18,970 |

Referencia "nunca alertar" (sin modelo): costo R$ 12,460.

## 5. Interpretación y selección del modelo

**Modelo A (sin PCA, C=1).** Accuracy 0.657, Precision 0.119, Recall 0.635, F1 0.201, Average Precision 0.146. Detecta 226 de 356 entregas tardías, pero de cada 100 alertas solo 12 son realmente tardías (Precision baja): `class_weight='balanced'` empuja al modelo a priorizar el Recall a costa de muchas falsas alarmas.

**Modelos con PCA (B, C, D).** Recall entre 0.601 y 0.607; F1 entre 0.184 y 0.186; Average Precision entre 0.123 y 0.124. Frente al modelo A, el mejor de ellos en AP (C) disminuye el AP en 0.023 y disminuye el Recall en 0.028.

**Efecto de `C` (modelos B, C y D).** Con PCA, el costo va de R$ 18,847 a R$ 18,970, un rango de R$ 123 (0.7 % del costo de B). La diferencia es pequeña: con ~24.500 pedidos de entrenamiento y pocas variables de entrada, el modelo tiene poco riesgo de sobreajuste, así que endurecer o relajar la regularización cambia poco el resultado. Los tres modelos con PCA son, en la práctica, equivalentes.

**¿Ayudó PCA?** En esta ejecución **no**: ninguna configuración con PCA supera al modelo A en costo (el mejor con PCA, B, cuesta R$ 18,847 frente a R$ 17,902 de A) y A también tiene mayor Accuracy, Recall, F1 y AP. Una explicación plausible (no comprobada aquí) es que PCA descarta 9.5% de la variación numérica y esa parte descartada contiene algo de señal útil para predecir retrasos, porque PCA ordena por variación y no por capacidad predictiva. Además, con solo 24 variables numéricas la regresión logística no necesita comprimirlas: PCA aporta poco beneficio de eficiencia (de 151 a 142 entradas al modelo) y cuesta en interpretabilidad, porque las componentes mezclan variables originales.

**Accuracy no basta.** La referencia "nunca alertar" tendría Accuracy 0.932, más alta que la de cualquiera de los cuatro modelos (0.640 a 0.657), pero Recall 0: no detectaría ninguna entrega tardía. Por eso la decisión se apoya en Recall, F1, AP y, sobre todo, en el costo.

**Advertencia sobre el valor del modelo.** Con estos costos didácticos y umbral 0,50, ninguna configuración supera a la referencia de "no alertar nunca" (R$ 12,460): el costo de las 1,669 falsas alarmas del modelo A (R$ 13,352) pesa más que el ahorro por detectar 226 entregas tardías. Esto no dice que el modelo sea inútil (tiene capacidad de ordenar pedidos por riesgo: ROC-AUC 0.702), sino que el **umbral 0,50 con pesos balanceados genera demasiadas falsas alarmas** para estos costos. Ajustar el umbral (con datos de validación) sería el siguiente paso natural; queda fuera de esta actividad, que fija el umbral en 0,50.

**Modelo seleccionado: A** (Sin PCA, C=1.0). Es el de menor costo en validación (R$ 17,902; el más caro, D, cuesta R$ 18,970). Sus errores se reparten así: 1,669 falsos positivos (R$ 13,352, 75 % del costo) y 130 falsos negativos (R$ 4,550). Aunque cada falso negativo cuesta 4.38 veces más que un falso positivo, el volumen de falsos positivos domina el costo. En esta ejecución el modelo de menor costo también es el de mayor Accuracy, pero eso es una coincidencia: el criterio de decisión es el costo, no la Accuracy. Los costos son supuestos académicos y la comparación se hizo con una sola partición de validación, sin intervalos de incertidumbre.

## 6. Actividad 4 - Prueba Kolmogorov-Smirnov


| Elemento | Valor |
|---|---|
| Variable comparada | `valor_flete` (entrenamiento vs. validación) |
| Prueba | `scipy.stats.ks_2samp` (dos muestras) |
| H0 | Ambas muestras provienen de la misma distribución |
| H1 | Las distribuciones son diferentes |
| Estadístico D | 0.0098 |
| Valor p | 0.7999 |
| Nivel de significancia (α) | 0.05 |
| Decisión | **No se rechaza H0** |

**Interpretación.** Como p = 0.7999 es mayor o igual que 0.05, **no se rechaza H0**: no hay evidencia suficiente para afirmar que `valor_flete` se distribuya distinto en entrenamiento y validación. La mayor distancia entre las curvas es D = 0.0098 (unos 0.98 puntos porcentuales de proporción acumulada), una diferencia compatible con la variación esperada por azar. Esto **no demuestra** que las distribuciones sean idénticas: solo que esta prueba no detectó diferencias.

**Limitaciones metodológicas.**

1. La prueba compara **solo una variable** (`valor_flete`); no garantiza que las particiones se parezcan en las demás variables ni en la relación entre variables (distribución conjunta).
2. `valor_flete` tiene muchos valores repetidos (empates) y una cola larga con valores extremos; el valor p de KS está pensado para variables continuas, por lo que con empates es una aproximación.
3. Es una prueba de **significancia**, no de relevancia práctica: con muestras grandes puede detectar diferencias minúsculas, y con muestras pequeñas puede no detectar diferencias importantes. No rechazar H0 no prueba igualdad.
4. Si se repitiera la prueba en muchas variables habría que corregir por comparaciones múltiples: con α = 0,05 se esperaría rechazar H0 por azar en ~5 % de ellas.


## 7. Conclusiones

1. **PCA.** Se necesitan 15 de 24 componentes para conservar al menos el 90% de la varianza numérica (acumulada: 90.46%).
2. **Comparación de modelos.** Menor costo en validación: modelo A (R$ 17,902); mayor costo: modelo D (R$ 18,970).
3. **PCA vs. sin PCA.** En esta ejecución PCA no mejoró el costo ni las métricas del modelo base de la semana 12.
4. **Parámetro C.** Entre C = 0,1 y C = 10 (con PCA) el costo varía R$ 123 (0.7 %), una diferencia pequeña.
5. **Costos.** Con los supuestos didácticos (FP = R$ 8, FN = R$ 35) la decisión se basa en el costo y no en la Accuracy. Referencia "nunca alertar": R$ 12,460.
6. **Prueba KS.** D = 0.0098, p = 0.7999, α = 0.05: no se rechaza H0. No se detectó diferencia en `valor_flete` entre entrenamiento y validación, lo que no prueba igualdad y solo cubre una variable.
7. **Alcance.** Todo se evaluó en validación; la partición de prueba sigue reservada para la evaluación final. Los costos son supuestos académicos, no cifras reales de Olist.
