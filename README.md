# Trabajo-Final-en-R
# Regresión lineal múltiple con datos WDI

La regresión lineal múltiple es un modelo estadístico que permite explicar una variable dependiente a partir de dos o más variables independientes. En este caso, la variable dependiente es la **esperanza de vida al nacer** (en años), siguiendo la estructura de la Clase 3 de la Profesora Lesly Flores.

## El modelo
$$
\text{vida}_i= \beta_0 + \beta_1 \cdot \log_{10}(\text{pibpc}_i) + \beta_2 \cdot \text{infl}_i + \varepsilon_i \tag{1}
$$

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


## Resultados del modelo creado para este ejercicio

### Matriz de correlación

![Matriz de correlación](figuras/Rplot02.png)

La matriz muestra tres relaciones entre las variables:

- `log_pibpc` ↔ `vida`: correlación positiva moderada (**r = 0.49**) — a mayor PIB per cápita (en escala log), mayor esperanza de vida.
- `log_pibpc` ↔ `infl`: correlación positiva débil (**r = 0.11**) — prácticamente sin relación lineal.
- `vida` ↔ `infl`: correlación negativa moderada-alta (**r = −0.61**) — a mayor inflación, menor esperanza de vida.

### Estimación por MCO

```r
lm(vida ~ log_pibpc + infl, data = datos_sim)
```

| Coeficiente | Estimado | Error estándar | t | p-valor |
|---|---:|---:|---:|---:|
| Intercepto ($\beta_0$) | 70.194 | 6.267 | 11.201 | < 0.001 |
| `log_pibpc` ($\beta_1$) | 5.809 | 1.529 | 3.799 | 0.0017 |
| `infl` ($\beta_2$) | −0.233 | 0.051 | −4.546 | 0.0004 |

**R² = 0.68 · R² ajustado = 0.637 · F(2,15) = 15.91 · p < 0.001**

### Interpretación

- **Intercepto (70.19):** cuando `log_pibpc` e `infl` son cero, la esperanza de vida estimada sería de ~70.2 años. No tiene interpretación sustantiva directa.
- **`log_pibpc` (β₁ = 5.81, p = 0.002):** por cada unidad que aumenta el logaritmo del PIB per cápita, la esperanza de vida sube en promedio **5.81 años**, manteniendo la inflación constante. El efecto es estadísticamente significativo.
- **`infl` (β₂ = −0.23, p < 0.001):** por cada punto porcentual adicional de inflación, la esperanza de vida cae en promedio **0.23 años**, manteniendo el PIB constante. El efecto es estadísticamente significativo y consistente con la hipótesis teórica.
- El modelo explica el **68% de la varianza** en esperanza de vida (R² = 0.68), lo cual es un ajuste robusto para datos simulados con n = 18.

> Los datos son simulados con fines académicos (`set.seed(123)`). Los coeficientes reproducen las relaciones teóricas esperadas por la Curva de Preston y la literatura sobre inflación y salud.
