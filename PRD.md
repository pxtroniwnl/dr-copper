# PRD: DR-COPPER (Análisis Predictivo y de Volatilidad del Cobre (Dr. Copper))

**Asignatura:** Modelos de Regresión y Series de Tiempo
**Institución:** Universidad Tecnológica de Bolívar
**Enfoque del Proyecto:** Data Engineering & Análisis Financiero

---

## 1. Visión del Producto
Desarrollar un pipeline integral de modelado de series de tiempo para los futuros del cobre (HG=F), integrando variables macroeconómicas (proxy Índice Dólar/UUP). El sistema anticipará tanto la media direccional de los precios como su volatilidad condicional en el panorama económico actual (2026). Este proyecto estructura una solución orientada a la ingeniería de datos para evaluar qué arquitectura ofrece la mayor aplicabilidad, interpretabilidad y robustez en entornos financieros y de gestión de riesgos reales.

---

## 2. Objetivos del Proyecto

### 2.1. Diagnóstico Estocástico
Caracterizar la serie temporal mediante pruebas formales de raíces unitarias (ADF/KPSS) y análisis de autocorrelación (ACF/PACF). Validar empíricamente la existencia de una caminata aleatoria (*Random Walk*) y la presencia de heterocedasticidad condicional (*volatility clustering*).

### 2.2. Modelado Híbrido y Comparativa Arquitectónica
Implementar y contrastar cuatro enfoques metodológicos para determinar su viabilidad en la industria:
* **SARIMAX (Estadística Clásica):** Captura de relaciones lineales y choques exógenos. Proporciona alta interpretabilidad estadística sobre cómo el dólar afecta al cobre.
* **Prophet (Descomposición):** Aislamiento de componentes estacionales y cambios estructurales. Ideal para proyecciones visuales y toma de decisiones a nivel gerencial.
* **LSTM (Deep Learning):** Red neuronal recurrente para identificar dependencias complejas y no lineales. Útil para arbitraje de alta frecuencia y trading algorítmico.
* **GARCH (Gestión de Riesgo):** Modelado de la varianza condicional ($\sigma_t^2$). A diferencia de los modelos anteriores que predicen el precio exacto, GARCH predice la magnitud de la fluctuación (el riesgo). Es el estándar en la industria real para el cálculo del *Value at Risk* (VaR).

### 2.3. Benchmarking Operativo
Establecer un modelo *Naïve* (donde $P_{t+1} = P_t$) como línea base. Cualquier modelo sofisticado deberá superar esta métrica para justificar su costo computacional y su implementación en un entorno de producción.

---

## 3. Requerimientos Funcionales y Entorno de Ejecución

### 3.1. Entorno de Desarrollo Interactivo
* **Ejecución en Jupyter Notebooks (`.ipynb`):** Toda la lógica de extracción, ingeniería de características, entrenamiento, evaluación y visualización **debe realizarse y documentarse estrictamente en archivos `.ipynb`**. Esto asegura la reproducibilidad del experimento, permite la ejecución por bloques y facilita la evaluación académica mediante la intercalación de celdas Markdown y código Python.

### 3.2. Pipeline de Datos
* **Data Ingestion Continua:** Extracción automatizada vía API (`yfinance`). Imputación de valores nulos garantizando la alineación temporal de *Business Days* entre el activo principal y la variable exógena.
* **Feature Engineering Avanzado:**
    * Generación de rezagos temporales (*lags*).
    * Transformación a retornos logarítmicos continuos (estrictamente necesario para el modelo GARCH).
    * Normalización mediante `MinMaxScaler` aislando el conjunto de entrenamiento para la red LSTM.

### 3.3. Estrategia de Validación (Backtesting)
* Implementación de partición cronológica rígida (*Time Series Split* u *Out-of-Time Sample*). Prohibido el uso de validación cruzada estándar (K-Fold) para preservar la secuencia temporal y evitar la fuga de información.

---

## 4. Stack Tecnológico
* **Lenguaje Base:** Python 3.12+ (Ejecutado en entorno Jupyter/Colab).
* **Extracción y Manipulación:** `yfinance`, `pandas`, `numpy`.
* **Estadística Clásica y Riesgo:** `statsmodels` (tests y ACF/PACF), `pmdarima` (Auto-ARIMA), `arch` (implementación de GARCH).
* **Machine Learning & Deep Learning:** `prophet`, `tensorflow` / `keras` (arquitectura LSTM).
* **Preprocesamiento y Métricas:** `scikit-learn` (RMSE, MAE, MAPE).
* **Gestor de Entorno/Paquetes:** `uv` (para resolución rápida de dependencias).

---

## 5. Roadmap de Ejecución (Cronograma de Fases)

| Fase | Archivo `.ipynb` | Descripción de Actividades | Estado |
| :--- | :--- | :--- | :---: |
| **1** | `01_eda_y_diagnostico.ipynb` | Extracción de datos (2000-2026), análisis de tendencia, estacionalidad y comprobación de efectos ARCH. | ✅ Completado |
| **2** | `02_data_prep.ipynb` | Alineación de series (HG=F y UUP), manejo de *missing values*, escalado y partición Train/Test. | ✅ Completado |
| **3** | `03_modelado_media.ipynb` | Entrenamiento, ajuste de hiperparámetros y predicción direccional usando SARIMAX, Prophet y LSTM. | ⏳ En Progreso |
| **4** | `04_modelado_varianza.ipynb`| Implementación de GARCH(1,1) sobre los retornos para predecir clústeres de volatilidad y estimar el riesgo. | ⏳ Pendiente |
| **5** | `05_backtesting_y_eval.ipynb` | Evaluación de los modelos contra el conjunto de prueba (2026) y test de Ljung-Box sobre los residuales. | ⏳ Pendiente |

---

## 6. Métricas de Éxito y Criterios de Aplicabilidad Real

El proyecto se considerará un éxito si cumple con los siguientes parámetros técnicos y de negocio:
1.  **Precisión en Media:** Los modelos (LSTM/SARIMAX/Prophet) logran reducir el RMSE de la predicción en al menos un **10%** respecto al modelo *Naïve*.
2.  **Robustez de los Residuales:** La prueba de Ljung-Box sobre los errores del modelo confirma la hipótesis de ruido blanco (p-value > **0.05**).
3.  **Captura del Riesgo:** El modelo GARCH modela con precisión la heterocedasticidad condicional detectada en la Fase 1.
4.  **Veredicto de Industria:** Se concluye con un análisis crítico que determine el caso de uso real para cada arquitectura (ej. la red LSTM es superior para predicciones direccionales complejas, pero el modelo GARCH es el enfoque definitivo y mandatorio para la gestión de capital y evaluación de riesgo en mesas de dinero).