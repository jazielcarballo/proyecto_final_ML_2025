# Detección Automatizada de Asentamientos Humanos en Imágenes Satelitales

## Objetivo
El objetivo de este proyecto es desarrollar un modelo de **clasificación binaria**, robusto y escalable, para detectar la presencia o ausencia de asentamientos humanos en imágenes satelitales.

---

## Conjunto de Datos
El dataset fue proporcionado por **INEGI** como parte de un concurso público de ciencia de datos.  

- **Entrenamiento:** 1.1 millones de muestras  
- **Prueba:** 120,000 muestras  
- **Parches de imagen:** 16x16 píxeles con **6 bandas espectrales**:
  - Azul (Blue)
  - Verde (Green)
  - Rojo (Red)
  - Infrarrojo cercano (NIR)
  - Infrarrojo de onda corta 1 (SWIR1)
  - Infrarrojo de onda corta 2 (SWIR2)

🔗 [Enlace al dataset](https://drive.google.com/drive/folders/1UgnUSvjYG3G9-1ZIqNnZuKAalWcfMGTe?usp=drive_link)  

Nota: El dataset puede contener **ruido y errores de etiquetado**, lo cual se consideró durante el preprocesamiento y la evaluación.

---

## Metodología

### Solución Inicial
1. **Exploración de datos**: revisión de la distribución de clases (0 = sin asentamiento, 1 = asentamiento).  
2. **Balanceo**: estrategias como *undersampling*, *oversampling* o **SMOTE**.  
3. **Extracción de características**: estadísticas y espectrales.  
4. **Entrenamiento de modelos clásicos**:
   - Random Forest  
   - Regresión Logística  
5. **Métricas de evaluación**:
   - **F1 Score** (balance entre precisión y exhaustividad)  
   - **Exactitud (Accuracy)**  
   - **Recall (Sensibilidad)**  

### Solución Avanzada
Entrenamiento sobre el dataset **desbalanceado**, sin aplicar técnicas de balanceo.  

Técnicas aplicadas:  
- Uso de `class_weight='balanced'`  
- Ajuste de umbral (trade-off precisión-recall)  
- Calibración de probabilidades  
- Métricas adicionales: **AUC-ROC**, F1 por clase minoritaria  

Se realizó un **análisis comparativo** entre los modelos entrenados con dataset balanceado vs desbalanceado.  

---

## Resultados
- Los modelos base obtuvieron métricas competitivas en el dataset balanceado.  
- Se observó mejora en el manejo del desbalance con **ponderación de clases** y ajuste de umbrales.  
- Se analizaron los trade-offs entre **precisión y recall** para aplicaciones reales.  

---

## Estructura del Repositorio

## Dependencias

El proyecto requiere Python 3.9+ y las siguientes librerías:
- scikit-learn
- pandas
- numpy
- matplotlib / seaborn
- jupyter
- imbalanced-learn
- opencv / scikit-image

## Referencias

- INEGI (Dataset del concurso de ciencia de datos)
- Solo Electrónicos, Cómo crear un dataset en el formato H5, 9 diciembre 2021. [En línea]. [Accedido: 9 agosto 2025].
- Documentación de Scikit-learn
- L. Breiman, Random forests, \textit{Machine Learning}, vol. 45, no. 1, pp. 5–32, 2001.
- A Wavelet Tour of Signal Processing, Academic Press, 2008.
- D. W. Hosmer, S. Lemeshow, and R. X. Sturdivant, Applied Logistic Regression, 3rd ed., Wiley, 2013.
