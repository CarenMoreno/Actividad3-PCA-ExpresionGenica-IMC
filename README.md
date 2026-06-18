# Actividad3-PCA-ExpresionGenica-IMC
Análisis bioestadístico de expresión génica y obesidad mediante PCA y regresión logística realizado en R. Proyecto de la asignatura Estadística y R para Ciencias de la Salud.

# Análisis bioestadístico de expresión génica asociado al IMC mediante PCA

# Descripción
Este repositorio contiene el análisis desarrollado para la Actividad 3 de la asignatura Estadística y R para Ciencias de la Salud.

Se estudiaron 37 genes relacionados con el metabolismo y la obesidad en 59 individuos utilizando técnicas de reducción de dimensionalidad mediante Análisis de Componentes Principales (PCA), clustering y modelos de regresión logística.

## Objetivos

- Explorar patrones de expresión génica.
- Reducir la dimensionalidad de los datos mediante PCA.
- Visualizar agrupamientos entre individuos y genes.
- Evaluar la asociación entre componentes principales y categorías de IMC.

## Estructura del repositorio

├── datos/
├── scripts/
├── resultados/
├── figuras/
├── tablas/
├── Actividad3.Rmd
├── Actividad3.html
└── README.md

# Principales resultados

# Proyecto Transversal en R (I): Análisis de bioestadística aplicada a la expresión génica en Los Simpson

**Materia:** Estadística y R para Ciencias de la Salud  
**Alumna:** Caren Moreno  
**Fecha:** 18 de junio de 2026  
**Dataset:** *Los Simpson* - 59 individuos, 37 genes asociados a obesidad

---

## ¿De qué trata este trabajo?

Este trabajo explora la relación entre la **expresión de 37 genes asociados a obesidad** y el **Índice de Masa Corporal (IMC)** en una población simulada de 59 individuos. Para eso usé un pipeline completo de análisis estadístico en R que va desde la carga y limpieza de los datos hasta la construcción de modelos de regresión logística, pasando por un PCA, heatmaps y tablas descriptivas.

La pregunta central es: ¿pueden los perfiles de expresión génica predecir si un individuo tiene sobrepeso (IMC ≥ 25)? Spoiler: la respuesta es moderadamente sí, pero con muchos matices biológicos interesantes en el camino.

---

## Estructura del repositorio

```
📁 Actividad3-PCA-ExpresionGenica-IMC/
│
├── 📄 Moreno_Caren_Actividad3.Rmd      # Código fuente en R Markdown
├── 📄 Moreno_Caren_Actividad3.html     # Informe final renderizado
├── 📄 README.md                        # Este archivo
│
├── 📁 data/
│   └── dataset_simpson.csv            # Dataset original
│
└── 📁 outputs/
    ├── figuras/                       # Gráficos exportados
    └── tablas/                        # Tablas de resultados
```

---

## Pipeline de análisis

El trabajo está organizado en **7 pasos secuenciales** que siguen una lógica estadística clara:

### Paso 1 - Carga y limpieza de los datos

Se cargó el dataset *Los Simpson* y se realizó una inspección inicial: valores faltantes, tipos de variables, distribución del IMC, etc. Se construyó la variable respuesta binaria `sobrepeso` (IMC ≥ 25 = 1, IMC < 25 = 0) para la regresión logística posterior.

---

### Paso 2 - Análisis de normalidad (test de Shapiro-Wilk)

Se evaluó la distribución de los 37 genes usando el test de Shapiro-Wilk. **Ninguno de los genes mostró distribución normal** (p < 0.05 en todos los casos). Este resultado no fue sorprendente: los datos de expresión génica rara vez son normales, especialmente en muestras pequeñas.

> **¿Por qué importa esto?** Define la estrategia estadística del resto del trabajo: se usan **medianas y rangos intercuartílicos** en las tablas descriptivas, y pruebas **no paramétricas** (Kruskal-Wallis) en las comparaciones entre grupos.

---

### Paso 3 - Análisis de Componentes Principales (PCA)

Este es el núcleo del trabajo. Con 37 genes y 59 individuos, el espacio de datos es de alta dimensionalidad. El PCA permite:

1. **Reducir la dimensionalidad** sin perder demasiada información
2. **Identificar patrones** de co-expresión génica
3. **Visualizar** la distribución de los pacientes en un espacio reducido

#### Varianza explicada

Las **6 primeras componentes** acumulan el **43.9% de la varianza total**. Puede parecer poco, pero es esperable en datos de expresión génica donde la señal biológica está distribuida entre muchos genes y la varianza es inherentemente ruidosa.

| Componente | Varianza (%) | Acumulada (%) |
|------------|-------------|---------------|
| PC1 | ~12% | ~12% |
| PC2 | ~9% | ~21% |
| PC3 | ~7% | ~28% |
| PC4 | ~6% | ~34% |
| PC5 | ~5% | ~39% |
| PC6 | ~5% | ~44% |

#### Interpretación biológica de los loadings

Los **loadings** (cargas) indican cuánto contribuye cada gen a cada componente. En PC1, los genes con cargas más altas tienden a ser aquellos asociados a procesos metabólicos inflamatorios y lipídicos, lo que tiene sentido dado que el dataset explora la obesidad.

En PC2 aparece una separación interesante que no responde al IMC de forma directa, posiblemente relacionada con otra fuente de variabilidad biológica (edad, sexo, u otra variable clínica).

---

### Paso 4 - Visualizaciones del PCA

Se generaron **5 visualizaciones complementarias** del PCA:

#### 4.1 Variables coloreadas según cos²

![Variables según cos²](outputs/figuras/pca_cos2.png)

El gráfico de círculo de correlaciones muestra qué tan bien representado está cada gen en el plano PC1-PC2. Los genes con **cos² alto** (color intenso) son los que el PCA captura mejor. Genes con cos² bajo están menos relacionados con las dos primeras componentes y quizás necesitarían más dimensiones para ser bien representados.

> **Interpretación:** Los genes bien representados en PC1-PC2 son los que más estructuran la variabilidad de expresión en nuestra muestra.

---

#### 4.2 Clustering de variables (k=3) sobre el espacio PCA

![Clustering de variables](outputs/figuras/pca_clustering_vars.png)

Se aplicó k-means (k=3) sobre las coordenadas de los genes en el espacio PCA. Los tres clusters identificados sugieren la existencia de **tres "programas" de expresión génica** relativamente independientes:

- **Cluster 1:** Genes con carga fuerte en PC1 (posiblemente genes de respuesta inflamatoria)
- **Cluster 2:** Genes con carga en PC2 (posiblemente genes metabólicos)
- **Cluster 3:** Genes con carga mixta o más dispersa

Esta agrupación es exploratoria pero biológicamente sugerente.

---

#### 4.3 Contribución de las variables a PC1 y PC2

![Contribución de variables](outputs/figuras/pca_contribuciones.png)

Las barras muestran qué porcentaje de la varianza de cada componente explica cada gen. La línea punteada indica el umbral esperado si todos los genes contribuyeran por igual (100/37 ≈ 2.7%). Los genes que superan ese umbral son los más informativos para cada componente.

---

#### 4.4 Distribución de pacientes en PC1-PC2 según categoría de IMC

![Individuos por IMC](outputs/figuras/pca_individuos_imc.png)

Este es uno de los gráficos más interesantes del trabajo. Los individuos se proyectan en el plano PC1-PC2 y se colorean según su categoría de IMC (normopeso, sobrepeso, obesidad).

> **Interpretación biológica:** Se observa una **tendencia de separación parcial** entre categorías de IMC a lo largo de PC1. Esto sugiere que los patrones de expresión génica capturados por PC1 tienen alguna relación con el IMC, aunque la separación no es perfecta. Dicho de otro modo: el IMC no está "escrito" de forma limpia en el transcriptoma, lo cual es coherente con la complejidad poligénica y multifactorial de la obesidad.

---

#### 4.5 Clustering de pacientes (k=3) según scores del PCA

![Clustering de pacientes](outputs/figuras/pca_clustering_pac.png)

Análogo al clustering de genes pero aplicado a individuos. Los tres clusters de pacientes no coinciden exactamente con las categorías de IMC, lo que refuerza la idea de que la expresión génica captura dimensiones de variabilidad que van más allá del peso corporal.

---

### Paso 5 - Heatmaps de correlaciones de Spearman

#### 5.1 Correlación entre genes y componentes principales

![Heatmap genes-componentes](outputs/figuras/heatmap_corr_genes_pc.png)

Este heatmap muestra la correlación de Spearman entre la expresión de cada gen y las 6 primeras componentes principales. Los asteriscos indican significancia estadística (*p<0.05, **p<0.01).

> **Interpretación:** La mayoría de los genes tiene correlación significativa con al menos una componente, pero raramente con más de dos. Esto confirma que el PCA está capturando señales biológicas relativamente diferenciadas en cada eje.

---

#### 5.2 Heatmap de expresión génica cruda por paciente

![Heatmap expresión cruda](outputs/figuras/heatmap_expresion_cruda.png)

La expresión génica (z-score por gen) de los 59 pacientes, ordenada por clustering jerárquico y anotada con la categoría de IMC. Se observan **bloques de co-expresión** que son parcialmente consistentes con el IMC pero no perfectamente concordantes.

> **Interpretación biológica:** El patrón de expresión sugiere que algunos genes se co-regulan en grupos, y que esos grupos tienen una asociación moderada con la categoría de IMC. La heterogeneidad dentro de cada categoría de IMC es notable, lo que apunta a que la obesidad tiene subtipos moleculares.

---

### Paso 6 — Tabla descriptiva estratificada por terciles del PCA

Las 3 primeras componentes principales se categorizan en terciles (T1=bajo, T2=medio, T3=alto) y se compara la expresión de cada gen entre grupos usando **Kruskal-Wallis** (justificado por la no-normalidad del Paso 2).

Esta tabla permite identificar qué genes se expresan de forma diferencial según la "posición" de los pacientes en el espacio PCA, conectando el análisis multivariante con la expresión individual de cada gen.

---

### Paso 7 — Regresión logística para sobrepeso (IMC ≥ 25)

#### 7.1 Identificación de variables confusoras

Se realizó un cribado sistemático de variables sociodemográficas y de estilo de vida. Se excluyeron deliberadamente variables que son parte constitutiva del IMC (peso, talla, perímetros, porcentaje de grasa) para evitar **colinealidad estructural** con la variable respuesta.

Una variable se incluye como confusora si su asociación cruda con el sobrepeso tiene p < 0.20 (criterio de Hosmer-Lemeshow).

#### 7.2 Modelos de regresión logística (niveles de ajuste progresivos)

Se construyeron **3 modelos anidados**:

| Modelo | Variables incluidas |
|--------|---------------------|
| Modelo 1 (crudo) | Solo PC1 (el componente con mayor varianza y asociación con IMC) |
| Modelo 2 (ajuste parcial) | PC1 + confusores clínicos identificados en 7.1 |
| Modelo 3 (ajuste completo) | PC1 + confusores clínicos + PC2 + PC3 |

#### 7.3 Odds Ratios e intervalos de confianza al 95%

Los OR del modelo final ajustado muestran que el score de PC1 tiene una asociación estadísticamente significativa con el sobrepeso, controlando por las variables de ajuste. Esto significa que el perfil de expresión génica capturado por PC1 tiene **valor predictivo independiente** para el estado de sobrepeso.

#### 7.4 Calidad de los modelos

Se evaluó mediante:
- **AIC/BIC**: para comparar parsimonia entre modelos
- **Test de Hosmer-Lemeshow**: para evaluar calibración
- **Curva ROC y AUC**: para evaluar discriminación

#### 7.5 Forest plot del modelo final

![Forest plot](outputs/figuras/forest_plot.png)

Representación visual de los OR con sus IC95%. Las variables cuyo intervalo no cruza el 1 son las que tienen asociación estadísticamente significativa con el sobrepeso.

#### 7.6 Comparación gráfica de los 3 modelos

![Comparación de modelos](outputs/figuras/comparacion_modelos.png)

---

## Conclusiones

El análisis de componentes principales sobre los 37 genes asociados a obesidad permitió reducir la dimensionalidad de los datos. Si bien las 6 primeras componentes explican conjuntamente un porcentaje moderado de la varianza total (~44%), esto es esperable dado el **carácter poligénico y multifactorial** de la obesidad.

Los principales hallazgos del trabajo son:

1. **Ningún gen tiene distribución normal** en la muestra (Shapiro-Wilk, p<0.05 en todos los casos), lo que justifica el uso de métodos no paramétricos en todo el pipeline.

2. **PC1 captura la mayor parte de la señal biológica** relacionada con el IMC. Los pacientes con obesidad tienden a ubicarse en valores más extremos de PC1, aunque la separación no es perfecta, lo que refleja la heterogeneidad biológica de la obesidad.

3. **El clustering de genes** identifica tres grupos de co-expresión que podrían corresponder a distintos programas biológicos (inflamatorio, metabólico y lipídico), aunque la validación funcional excede el alcance de este trabajo.

4. **La regresión logística con PC1 como predictor** muestra una asociación estadísticamente significativa con el sobrepeso (IMC ≥ 25) que se mantiene tras el ajuste por confusores. Esto sugiere que los patrones de expresión génica tienen **valor predictivo independiente** para el estado ponderal.

5. La **heterogeneidad dentro de cada categoría de IMC** observada en los heatmaps apoya la hipótesis de que la obesidad tiene subtipos moleculares distinguibles por expresión génica, un área activa de investigación en medicina de precisión.

---

## Tecnologías y paquetes utilizados

```r
# Análisis estadístico y PCA
library(tidyverse)
library(FactoMineR)
library(factoextra)

# Visualización
library(ggplot2)
library(pheatmap)
library(corrplot)

# Modelos y tablas
library(gtsummary)
library(tableone)
library(broom)
library(pROC)
library(forestplot)

# Formato del informe
library(rmarkdown)
library(knitr)
```

---

## Cómo reproducir el análisis

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/CarenMoreno/Actividad3-PCA-ExpresionGenica-IMC.git
   cd Actividad3-PCA-ExpresionGenica-IMC
   ```

2. Abrir `Moreno_Caren_Actividad3.Rmd` en RStudio

3. Hacer click en **Knit** → **Knit to HTML**

> **Requisito:** R ≥ 4.2.0 con los paquetes listados arriba instalados.

---

## Licencia

Este trabajo es de uso académico en el marco de la materia *Estadística y R para Ciencias de la Salud*. El dataset *Los Simpson* es un conjunto de datos simulado con fines educativos.

---

*Caren Moreno · Junio 2026*

