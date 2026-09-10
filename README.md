# Pronóstico de Demanda Eléctrica en Ecuador

Sistema de pronóstico de demanda eléctrica mensual en Ecuador (2020–2024) mediante la comparación de tres modelos: SARIMA, Prophet y LSTM. Trabajo de Titulación (Artículo Académico) — Ingeniería en Ciencias de la Computación, Universidad Politécnica Salesiana, Sede Guayaquil.

## Contexto

El estiaje severo de 2024 provocó cortes de hasta 14 horas continuas en Ecuador, evidenciando la falta de herramientas predictivas robustas frente a la alta dependencia hidroeléctrica del país (69% de la matriz energética). Este proyecto compara tres enfoques de forecasting sobre datos oficiales de demanda del CENACE para establecer una línea base de referencia reproducible.

## Resultados

| Modelo | MAE (GWh) | RMSE (GWh) | MAPE (%) |
|---|---|---|---|
| SARIMA (0,2,2)(1,1,1,12) | 358.66 | 440.16 | 16.20 |
| Prophet (aditivo) | 352.20 | 439.09 | 15.97 |
| **LSTM (32 unidades, dropout 0.2)** | **197.35** | **285.08** | **9.16** |

El modelo **LSTM** obtuvo el mejor desempeño en las tres métricas, con una reducción de error del 45% (MAE) frente a los modelos estadísticos clásicos. En meses de operación normal alcanzó un MAPE de ~2.9%. La superioridad fue validada estadísticamente con la prueba de Wilcoxon (p < 0.05 vs. SARIMA y Prophet).

Los tres modelos, al ser univariados, fallaron en anticipar la caída abrupta de demanda durante el estiaje (sep–dic 2024), ya que esta respondió a restricciones de oferta y no a un patrón interno de consumo.

## Stack técnico

- **Python** — pandas, numpy
- **statsmodels** — SARIMA, pruebas Dickey-Fuller y KPSS
- **Prophet** (Meta) — modelado de tendencia/estacionalidad
- **TensorFlow/Keras** — red LSTM
- **scipy** — prueba de Wilcoxon
- **matplotlib** — visualizaciones

## Datos

60 registros mensuales oficiales del CENACE (enero 2020 – diciembre 2024), medidos en GWh, extraídos y validados manualmente desde los informes anuales de gestión operativa.

## Estructura del repositorio

| Archivo | Contenido |
|---|---|
| `00_preprocesamiento.ipynb` | Carga de datos, análisis exploratorio, pruebas de estacionariedad, descomposición estacional, partición cronológica, escalado |
| `01_arima_sarima.ipynb` | Búsqueda en grilla, ajuste SARIMA, diagnóstico de residuos, predicción |
| `02_prophet.ipynb` | Tuning de hiperparámetros, ajuste Prophet, predicción |
| `03_lstm.ipynb` | Tuning de hiperparámetros, entrenamiento LSTM (5 semillas), predicción |
| `04_comparacion_final.ipynb` | Consolidación de resultados, tablas y gráficos comparativos |
| `demanda_mensual_SNI_2020_2024.csv` | Dataset utilizado |
| `tabla_predicciones_completa.csv` | Predicciones mensuales detalladas de los 3 modelos |

## Cómo ejecutar

1. Abrir los notebooks en [Google Colab](https://colab.research.google.com/) o Jupyter local
2. Ejecutar en orden secuencial (00 → 04); cada notebook depende de los artefactos generados por el anterior
3. Instalar dependencias: `pip install pandas numpy statsmodels prophet tensorflow scipy matplotlib`

## Autor

Cristhian Andrés Sánchez Cevallos — Tutor: Freddy Javier Tejada Escobar
Universidad Politécnica Salesiana, Sede Guayaquil, 2026
