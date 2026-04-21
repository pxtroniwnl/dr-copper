# Análisis de la Dinámica Estocástica del Cobre (High Grade Copper) Dr. Copper

Este repositorio contiene el desarrollo técnico del análisis de series de tiempo para los precios de futuros del cobre (HG=F). El estudio se centra en la caracterización estadística, validación de estacionariedad e identificación de estructuras de dependencia no lineal en el contexto económico de 2026.

## 📌 Justificación y Contexto
El cobre, tradicionalmente conocido como **"Dr. Copper"**, es un indicador líder de la salud económica mundial. En el panorama actual de 2026, su relevancia se ha intensificado debido a:

1.  **Infraestructura de IA:** Componente esencial en semiconductores (interconexiones y pilares de sustrato) y sistemas de gestión térmica en centros de datos.
2.  **Transición Energética:** Metal crítico para la fabricación de vehículos eléctricos y la expansión de redes de energía renovable.
3.  **Indicador Macro:** Su volatilidad es un termómetro de la inflación industrial y el crecimiento manufacturero global.

## 🛠️ Fase de Diagnóstico Estadístico
El proyecto ha completado las etapas de validación necesarias para el modelado formal:

* **Análisis Exploratorio (EDA):** Identificación de una tendencia ascendente no lineal y componentes estacionales mediante descomposición STL.
* **Pruebas de Estacionariedad:** * **Serie en Niveles:** La prueba de Dickey-Fuller Aumentada (ADF) confirmó la no-estacionariedad de la serie original.
    * **Serie Diferenciada:** Tras aplicar la primera diferencia ($d=1$), la serie se transformó en un proceso estacionario en media.
* **Análisis de Correlación:**
    * **ACF y PACF:** Los correlogramas de la serie diferenciada sugieren un comportamiento de **Caminata Aleatoria (Random Walk)**, indicando eficiencia en el mercado de media.
    * **Dependencia en Cuadrados:** El análisis de los residuos al cuadrado reveló la presencia de **Heterocedasticidad Condicional**, justificando la necesidad de modelos de varianza.



## 🚀 Tecnologías y Herramientas
* **Lenguaje:** Python 3.x
* **Librerías de Ciencia de Datos:**
    * `yfinance`: Extracción de datos financieros históricos.
    * `statsmodels`: Diagnóstico estadístico y descomposición de señales.
    * `pandas` & `numpy`: Procesamiento de estructuras de datos temporales.
    * `matplotlib` & `plotly`: Visualización de dinámicas de precios y volatilidad.

## 📂 Estructura del Análisis
* **Tendencia:** Análisis de movimientos estructurales a largo plazo (2000-2026).
* **Estacionalidad:** Captura de ciclos anuales vinculados a la demanda industrial.
* **Varianza:** Identificación de *clusters* de volatilidad en periodos de crisis y expansiones tecnológicas.



## 👨‍💻 Autor
**Alejandro Patron Montero**
Estudiante de Ciencia de Datos / Ingeniería de Sistemas
Universidad Tecnológica de Bolívar (UTB)
Cartagena de Indias, Colombia, 2026
