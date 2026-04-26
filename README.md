# ¿Cuánto deberías cobrar? Un modelo que te lo explica
Predicción salarial end-to-end con explicabilidad total vía SHAP.

## Resumen

Este proyecto construye un sistema de predicción salarial capaz de responder con precisión y argumentación matemática a una de las preguntas más habituales — y más difíciles de responder — en la carrera profesional: **¿cuánto debería cobrar alguien con mi perfil?**

Entrenado sobre **250.000 observaciones**, el modelo no solo predice el salario de mercado esperado, sino que te permite argumentar cada predicción variable por variable, convirtiendo un algoritmo  de caja negra en una herramienta práctica que cualquier profesional puede utilizar para argumentar su valor de mercado o detectar si está siendo infravalorado.

**Hallazgos principales:**

1. **El Feature Engineering fue el factor diferencial.** Las dos variables con mayor poder predictivo son de nueva creación: `edu_investment_index` y `exp_x_edu` superan en señal estadística a todas las variables originales del dataset.
2. **Tres ejes estructurales determinan el salario.** El contexto educativo-geográfico (`edu_investment_index`, $14.514 de impacto medio), el tamaño de empresa (`company_size`, $13.422) y la ubicación geográfica (`location_tier`, $7.994) concentran la mayor parte del poder predictivo. Optimizar estos tres factores puede suponer más de $100.000 anuales de diferencia respecto a la media.
3. **La experiencia no es lo más importante.** `experience_years` ocupa la quinta posición en el ranking SHAP — por detrás de dos features construidas a partir de ella. La experiencia bruta importa menos que su interacción con educación y habilidades.
4. **La infravaloración salarial es cuantificable.** El diagnóstico individual desarrollado en el proyecto demuestra que la brecha entre el salario actual y el valor de mercado real puede superar el 40% en perfiles competitivos — y es argumentable con un número exacto.

## Contenido del repositorio

- `salary_predictor_explainable_ml.ipynb` — Notebook con el análisis completo (EDA, Feature Engineering, modelado, SHAP, simulador, diagnóstico).
- `job_salary_prediction_dataset.csv` — Base de datos utilizada (250.000 observaciones, 10 variables).
- `README.md` — Este archivo.

## Metodología

El proyecto sigue el flujo completo de un problema de regresión supervisada con énfasis en explicabilidad e impacto práctico:

1. **EDA completo:** estadísticos descriptivos, distribuciones, correlaciones de Pearson, Spearman y Mutual Information, análisis bivariante contra el target y detección de outliers.
2. **Feature Engineering:** creación y validación de 16 variables derivadas (interacciones, ratios, índices compuestos e indicadores binarios) validadas con Mutual Information frente a las originales.
3. **Modelado con cuatro algoritmos** (ElasticNet, KNN, RandomForest, GradientBoosting) via GridSearchCV con validación cruzada y análisis de sobreajuste.
4. **Selección del modelo final** por rendimiento predictivo y estabilidad train-test (GradientBoosting, R²=0.916, sobreajuste=0.009).
5. **Explicabilidad completa vía SHAP:** Bar Plot (importancia global), Beeswarm (dirección del impacto), Dependence Plot (relaciones no lineales) y Waterfall (desglose individual por perfil).
6. **Simulador de salario end-to-end:** función que predice el salario de mercado de cualquier perfil y desglosa en lenguaje natural los factores que lo impulsan y lo limitan.
7. **Diagnóstico individual:** dado el salario actual, determina si el profesional está infravalorado, alineado o sobrevalorado — con la brecha exacta en dólares y porcentaje.

## Resultados del modelo final

| Métrica | Train | Test |
|---|---|---|
| R² | 0.925 | 0.916 |
| MAE | — | $8.085 |
| RMSE | — | $10.813 |
| % error < 10% | — | 83.2% |
| % error < 5% | — | 55.8% |

Diferencia train-test de 0.009 en R²: sin evidencias de sobreajuste relevante.

## Stack técnico

- Python 3.10+
- pandas, numpy, scipy
- scikit-learn (Pipelines, ColumnTransformer, GridSearchCV)
- matplotlib, seaborn
- shap

## Cómo reproducir el análisis

```bash
# Clonar el repositorio
git clone https://github.com/yeraybc/salary-predictor-explainable-ml.git
cd salary-predictor-explainable-ml

# Instalar dependencias
pip install pandas numpy scipy scikit-learn matplotlib seaborn shap jupyter

# Abrir el notebook
jupyter notebook salary_predictor_explainable_ml.ipynb
```

## Limitaciones reconocidas

- Base de datos sintética: las conclusiones cuantitativas no deben extrapolarse a mercados laborales reales sin validación adicional.
- Granularidad geográfica a nivel de país: diferencias salariales internas (ciudad, región) no están capturadas.
- Ausencia de variables macroeconómicas y temporales: el modelo es una foto estática, no contempla inflación ni ciclos de mercado.
- Predicciones puntuales sin intervalo de confianza asociado.

Estas limitaciones están detalladas en la sección K del notebook.

## Autor

**Yeray Benito Calviño**  
Estudiante de 3º Data Science — Universidad Complutense de Madrid  
[LinkedIn] · [GitHub]

## Licencia

Este proyecto se publica con fines educativos y de portafolio. La base de datos utilizada es de dominio público (Kaggle).