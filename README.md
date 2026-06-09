# Trabajo-Final-en-R
# Regresión lineal múltiple con datos WDI

La regresión lineal múltiple es un modelo estadístico que permite explicar una variable dependiente a partir de dos o más variables independientes. En este caso, la variable dependiente es la **esperanza de vida al nacer** (en años), siguiendo la estructura de la Clase 3 de la Profesora Lesly Flores.

## El modelo
\[
\text{vida}_i = \beta_0 + \beta_1 \cdot \log_{10}(\text{pibpc}_i) + \beta_2 \cdot \text{infl}_i + \varepsilon_i
\]

| Variable | Descripción |
|---|---|
| `vida` | Esperanza de vida al nacer (años) |
| `pibpc` | PIB per cápita en USD corrientes |
| `log_pibpc` | Logaritmo decimal del PIB per cápita: $\log_{10}(\text{pibpc})$ |
| `infl` | Inflación anual (%), medida como variación del IPC |
| $\beta_0$ | Intercepto |
| $\beta_1, \beta_2$ | Coeficientes de cada predictor |
| $\varepsilon_i$ | Error aleatorio |

## ¿Por qué usar el logaritmo del PIB per cápita?

El ingreso tiene una relación **no lineal** con la esperanza de vida: en niveles bajos, un aumento en el ingreso eleva la esperanza de vida rápidamente; en niveles altos, el efecto se atenúa. Esto se conoce como la **Curva de Preston**. Al aplicar $\log_{10}$, la relación se linealiza y el modelo ajusta mejor.

## Interpretación de los coeficientes

- $\beta_1 > 0$ → a mayor PIB per cápita (en escala log), mayor esperanza de vida.
- $\beta_2 < 0$ → a mayor inflación, menor esperanza de vida (la inflación alta se asocia a inestabilidad económica y menor acceso a salud).
- Un coeficiente con **valor p < 0.05** se considera estadísticamente significativo.

## Objetivo

Con datos simulados de **18 países latinoamericanos**, este trabajo estima el modelo (1) en R siguiendo los pasos de la Clase 3:

1. Simulación de los indicadores WDI
2. Limpieza de datos (eliminación de `NA`)
3. Creación de `log_pibpc`
4. Cálculo de correlaciones
5. Ajuste del modelo con `lm()`
6. Interpretación de `summary()`
7. Gráfico de la relación entre `log_pibpc` y `vida` con línea de regresión

> **Nota:** Los datos utilizados son simulados con fines académicos para reproducir la estructura metodológica de la Clase 3, no datos WDI oficiales.
