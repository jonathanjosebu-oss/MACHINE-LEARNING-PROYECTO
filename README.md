# QSRR Prediction using Machine Learning and RepoRT Dataset

Proyecto desarrollado para la asignatura de **Aprendizaje de Máquina (Machine Learning)** de la Maestría en Ciencia de Datos (MSDS) de la Universidad San Francisco de Quito (USFQ).

---

## 1. Contexto y Descripción del Problema
Este proyecto aborda un problema de **QSRR (Quantitative Structure-Retention Relationship)** utilizando datos masivos extraídos del repositorio público **RepoRT**. El objetivo central es predecir el **tiempo de retención ($rt$)** de compuestos químicos en cromatografía líquida (HPLC) a partir de sus descriptores moleculares, automatizando el flujo metodológico completo de Machine Learning dentro de un entorno reproducible en Google Colab y sin la necesidad de un informe externo adicional.

---

## 2. Estructura y Explicación del Código Implementado

El flujo de trabajo implementado en la notebook se divide en los siguientes módulos principales:

* **Carga y Consolidación de Datos:** Se clonó el repositorio de RepoRT y se automatizó la búsqueda e indexación mediante glob, fusionando archivos de datos de retención (rtdata) con sus respectivos descriptores moleculares utilizando el identificador único (id) a escala masiva (miles de moléculas).
* **Preprocesamiento y Limpieza:** Se implementó imputación de valores faltantes (SimpleImputer con estrategia de mediana) y un filtro de varianza (VarianceThreshold) para eliminar descriptores constantes o sin variabilidad informativa.
* **Pipeline Modular de Scikit-Learn:** Se construyó un Pipeline robusto que encapsula los transformadores y el estimador final (RandomForestRegressor), previniendo la fuga de datos (*data leakage*) durante la validación cruzada.
* **Validación Cruzada Repetida y Optimización:** Se configuró validación cruzada repetida (RepeatedKFold con n_splits=5 y n_repeats=3) con un random_state=42 fijo para garantizar la reproducibilidad científica de los resultados.
* **Comparación Estadística (Test de Wilcoxon):** Se contrastó el rendimiento del modelo principal frente a un baseline (Regresión Ridge) evaluando las diferencias fold a fold mediante el test de hipótesis de Wilcoxon con un nivel de significancia $\alpha = 0.05$.
* **Reducción Dimensional (t-SNE):** Se proyectó el espacio de alta dimensionalidad de los descriptores químicos a un plano bidimensional mediante t-SNE para visualizar la agrupación y transición de los tiempos de retención.

---

## 3. Resultados Clave y Evaluación

* **Métricas de Regresión:** El modelo basado en ensamblajes de árboles (Random Forest) logró un rendimiento competitivo en la predicción del tiempo de retención, evaluado mediante el Error Cuadrático Medio (MSE), RMSE y el coeficiente de determinación ($R^2$).
* **Optimización e Hiperparámetros:** Se analizaron curvas de optimización variando la complejidad del modelo (profundidad de los árboles / hiperparámetro de regularización), identificando el punto óptimo de generalización ("*just right*").

---

## 4. Conclusiones Relevantes

1. **Diagnóstico de Sobreajuste (Overfitting):** Al analizar las curvas de aprendizaje y de pérdida ($MSE$ vs. tamaño de muestra $m$), se evidenció una brecha significativa (*gap*) entre el rendimiento de entrenamiento y el de validación cruzada ($R^2$ alto en entrenamiento vs. moderado en validación). Esto demuestra que el modelo presenta alta varianza debido a la gran flexibilidad del *Random Forest* combinada con la alta dimensionalidad del espacio químico.
2. **Impacto del Tamaño de Muestra ($m$):** A pesar del sobreajuste inicial, las curvas de aprendizaje demostraron que al incrementar el volumen de datos de entrenamiento (hacia los ~3,000 registros del dataset RepoRT), el error de validación cruzada presenta una tendencia descendente constante. Esto comprueba que **incorporar mayor diversidad de estudios cromatográficos mejora de forma directa la capacidad de generalización** del modelo QSRR.
3. **Mitigación y Trabajos Futuros:** Se concluye que para mejorar el desempeño predictivo en producción, es necesario restringir la complejidad del algoritmo (ej. limitando max_depth o aumentando min_samples_leaf) y continuar escalando la integración de nuevos estudios cromatográficos desde el repositorio RepoRT.

---

## 5. Referencias (IEEE)

* [1] A. Grbić et al., *"RepoRT: A repository of chromatographic retention data"*, Journal of Chromatography A, 2021.
* [2] F. Pedregosa et al., *"Scikit-learn: Machine Learning in Python"*, Journal of Machine Learning Research, vol. 12, pp. 2825–2830, 2011.
* [3] Asistencia técnica en fragmentos de código e interpretación de curvas de aprendizaje apoyada mediante modelos de lenguaje (OpenAI / Google Gemini), documentada inline en los comentarios del código fuente.

---

## 6. Archivos del Proyecto

- PoryectoMLmasdatos.ipynb: notebook principal con la implementación completa del flujo de trabajo.
- README.md: documentación del proyecto.

## 7. Cómo usar este proyecto

1. Abrir la notebook PoryectoMLmasdatos.ipynb en Google Colab o Jupyter Notebook.
2. Ejecutar las celdas en orden para cargar los datos, preparar el pipeline, entrenar el modelo y evaluar resultados.
3. Revisar las métricas y conclusiones obtenidas durante la validación cruzada y la comparación estadística.

