# Proyecto: Detección automatizada de asentamientos humanos a partir de imágenes satelitales

Este proyecto desarrolla un modelo de **clasificación binaria** para detectar la **presencia o ausencia de asentamientos humanos** en imágenes satelitales Landsat mediante **machine learning**. Dicho proyecto forma parte de la asignatura Machine Learning dirigida por el Dr. Hugo Carlos Martínez, del Posgrado Integrado en Ciencias de Información Geoespacial de [CentroGeo](https://www.centrogeo.org.mx/).

---

## Objetivo
El objetivo es construir un modelo robusto y escalable que pueda identificar asentamientos humanos a partir de imágenes satelitales, considerando tanto escenarios balanceados como desbalanceados.

---

## Dataset
El conjunto de datos fue obtenido de datos del **INEGI** en un reto de ciencia de datos:  
📂 [Google Drive Dataset](https://drive.google.com/drive/folders/1UgnUSvjYG3G9-1ZIqNnZuKAalWcfMGTe?usp=drive_link)

- **Training set**: 1.1 millones de muestras  
- Cada muestra es un parche de **16x16 píxeles** con **6 bandas espectrales**:
  - Azul (Blue)  
  - Verde (Green)  
  - Rojo (Red)  
  - Infrarrojo cercano (NIR)  
  - Infrarrojo de onda corta 1 (SWIR1)  
  - Infrarrojo de onda corta 2 (SWIR2)  

El dataset incluye **ruido e incertidumbre**, propios de la teledetección y el etiquetado manual.

---

## Solución Inicial (Balanced)
Pasos principales:
1. Exploración y análisis de la distribución de clases.  
2. Aplicación de undersampling como estrategia de balanceo.  
3. Extracción de **features estadísticos/espectrales**. Para cada muestra,
se calcularon características tanto en el dominio espacial
como en el dominio de la frecuencia. En el dominio de
la frecuencia se utilizó la Transformada Rápida de Fourier (FFT). 
4. Entrenamiento con modelos tradicionales (**Random Forest y Logistic Regression**).  
5. Evaluación con métricas:
   - **Accuracy**  
   - **F1 Score**  
   - **Recall**  

---

## Solución Avanzada (Imbalanced)
En esta etapa se entrena con el **dataset original desbalanceado**. Se implementaron técnicas para mitigar el sesgo:
- Pesos balanceados en los modelos (`class_weight="balanced"`)  
- Ajuste de umbrales de decisión  
- Calibración de probabilidades  
- Uso de métricas adicionales: **AUC-ROC, F1 minoría**  

Incluye un **análisis comparativo** entre el enfoque balanceado y el desbalanceado, destacando trade-offs entre recall y precisión.  

---

## Resultados
### Desempeño de la Regresión Logística antes y después de la normalización

| Condición              | Accuracy | F1 Score | Recall |
|------------------------|----------|----------|--------|
| Antes de normalizar    | 0.5696   | 0.56     | 0.57   |
| Después de normalizar  | 0.77     | 0.77     | 0.77   |

### Comparación de desempeño entre modelos en el conjunto de validación

| Modelo         | F1     | Exactitud | Recall  |
|----------------|--------|-----------|---------|
| Random Forest  | 0.8054 | 0.8057    | 0.8038  |
| Reg. Logística | 0.77   | 0.77      | 0.77    |

---

### Importancia de variables en el modelo **Random Forest**
1. Mediana: **0.2521**  
2. FFT mean: **0.1196**  
3. Curtosis: **0.1193**  
4. Dispersión: **0.1108**  

---

### Matriz de confusión del modelo **Random Forest**

|            | Pred 0 | Pred 1 |
|------------|--------|--------|
| **Real 0** | 14,722 |  5,278 |
| **Real 1** |  4,867 | 15,133 |

### Análisis del desempeño de la Regresión Logística con y sin normalización

El reporte de clasificación **sin normalización** evidenció un **sesgo hacia la clase positiva (asentamiento)**, logrando un **recall de 0.70 en la clase 1** pero sacrificando precisión y desempeño general, con un **accuracy global de 0.57**.  
Esto indica que, sin normalización, el modelo favorece la detección de positivos pero genera **muchos falsos positivos**, afectando la confiabilidad de las predicciones.

En contraste, al aplicar **normalización**, el modelo alcanzó un desempeño equilibrado con métricas cercanas a **0.77 en ambas clases** (accuracy, F1 Score y recall), mostrando un comportamiento mucho más **robusto y consistente**.  
La normalización permitió que todas las variables contribuyeran de manera proporcional al aprendizaje, evitando que características con magnitudes mayores dominaran el entrenamiento.

Estos resultados confirman la importancia de la **normalización en modelos lineales como la Regresión Logística**, ya que facilita un aprendizaje más estable y mejora la capacidad predictiva del clasificador.

---


##  Requisitos
- Python 3.9+  
- Librerías:
  - `numpy`, `pandas`, `scikit-learn`, `scipy`
  - `matplotlib`, `seaborn`
  - `h5py`
  - `jupyter`  

## Instrucciones de instalació0n

Sigue estos pasos para configurar el proyecto en tu máquina local:

1. **Clonar el repositorio**
   ```bash
   git clone https://github.com/jazielcarballo/proyecto_final_ML_2025.git
   cd <proyecto_final_ML_2025>
   
## Como reproducir los resultados obtenidos

Sigue estos pasos para reproducir los resultados obtenidos en el proyecto:

1. **Preparar el entorno**
   - Clonar el repositorio y crear un entorno virtual.  
   - Instalar las dependencias listadas en `requirements.txt`.  
   ```bash
   git clone https://github.com/<usuario>/<nombre-repo>.git
   cd <nombre-repo>
   pip install -r requirements.txt

2. **Descargar y organizar el dataset**

 - Obtener el dataset desde el enlace oficial:
📂 [Google Drive Dataset](https://drive.google.com/drive/folders/1UgnUSvjYG3G9-1ZIqNnZuKAalWcfMGTe?usp=drive_link)
 - Colocar los archivos en la carpeta data/.

3. **Ejecutar la solución inicial (balanced dataset)**
  - Al finalizar, se generarán métricas de Accuracy, F1 Score y Recall para los modelos entrenados.

4. **Ejecutar la solución avanzada**
5. **Generar reportes y visualizaciones**
6. **Resultados esperados**
  - Random Forest:
F1 ≈ 0.8054, Accuracy ≈ 0.8057, Recall ≈ 0.8038
  - Regresión Logística (normalizada):
F1 ≈ 0.77, Accuracy ≈ 0.77, Recall ≈ 0.77

## Autores

- Katia M. Villarnobo G.
- Jaziel Carballo-Tadeo

## Referencias

- INEGI (Dataset del concurso de ciencia de datos)
- Solo Electrónicos, Cómo crear un dataset en el formato H5, 9 diciembre 2021. [En línea]. [[Accedido: 9 agosto 2025](https://soloelectronicos.com/2021/12/09/como-crear-un-dataset-en-el-formato-h5/)].
- Documentación de Scikit-learn
- L. Breiman, Random forests, \textit{Machine Learning}, vol. 45, no. 1, pp. 5–32, 2001.
- A Wavelet Tour of Signal Processing, Academic Press, 2008.
- D. W. Hosmer, S. Lemeshow, and R. X. Sturdivant, Applied Logistic Regression, 3rd ed., Wiley, 2013.

