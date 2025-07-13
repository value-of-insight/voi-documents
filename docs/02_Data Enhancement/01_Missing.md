---
sidebar_position: 1
---

# 🧹 Módulo de Imputación de Missings

Este módulo forma parte del proceso integral de *data enhancement* y tiene como objetivo principal **corregir los valores faltantes en un conjunto de datos**, priorizando la preservación de filas relevantes (clientes) y aplicando técnicas avanzadas de imputación a las columnas críticas definidas por el usuario.

---

## 🎯 Objetivo general

Detectar y corregir valores faltantes (`NaN`, `None`, `pd.NA`) siguiendo una lógica basada en umbrales, relevancia de columnas y técnicas estadísticas avanzadas como **KNNImputer** e **IterativeImputer**.

---

## 📊 Criterios de decisión por fila

Cada fila del dataset es evaluada según el porcentaje de valores faltantes y se aplica una acción diferente:

| % de Faltantes | Acción | Descripción |
|----------------|--------|-------------|
| **1% – 20%**   | ✅ Imputación automática | Se usan KNN o IterativeImputer dependiendo del tamaño del dataset |
| **21% – 44%**  | 🔍 Evaluación manual | Se revisa si al menos el **60% de las columnas críticas están completas** |
| **> 45%**      | ❌ Eliminación | Se considera que aporta poco valor informativo |

> ⚠️ *Las columnas críticas son definidas por el usuario según su importancia para el modelo final.*

---

## 🧰 Técnicas de imputación

### 1. **KNN Imputer (vecinos más cercanos)**

- **Aplicado:** solo columnas numéricas
- **Uso recomendado:** datasets pequeños (hasta 5000 filas)
- **Ventaja:** captura relaciones locales entre registros

> 🔩 Este método calcula distancias entre muestras, encuentra los K vecinos más similares y usa la media o mediana de esos vecinos para imputar.
>
> **Fórmula de distancia euclidiana:**
> 
> $$
d(x, y) = \sqrt{\sum_{i=1}^{n} (x_i - y_i)^2}
$$

---

### 2. **IterativeImputer**

- **Aplicado:** solo columnas numéricas
- **Uso recomendado:** datasets grandes (> 5000 filas)
- **Ventaja:** modela cada característica como función de las demás (muy preciso)

> 🔩 Usa el modelo de regresión **BayesianRidge** y se repite la imputación varias veces, refinando los valores en cada paso.

> 💡 Internamente funciona como una regresión múltiple iterativa: cada variable con missings se predice usando las demás.

---

> ⚠️ **Nota importante:** Ambos imputadores pueden aplicarse solo sobre ciertas columnas definidas como críticas por el usuario.

---

## 📁 Estructura del Paquete Imputación Missings

La estructura modular del paquete permite fácil mantenimiento, expansión y reutilización. Está compuesta por los siguientes archivos:

1. **`base.py`**: Interfaz base para imputadores (`MissingImputerBase`)
2. **`knn_imputer.py`**: Wrapper de `KNNImputer`
3. **`iterative_imputer.py`**: Wrapper de `IterativeImputer`
4. **`strategy_selector.py`**: Decide qué imputador usar según el tamaño del dataset
5. **`missing_processor.py`**: Controlador principal del flujo de imputación
6. **`report_printer.py`**: Genera reporte visual del resultado

---

## 🧩 Flujo del módulo

### Paso 1: Definición de columnas relevantes
El usuario define cuáles son las columnas críticas para el análisis o modelo final (ej: `edad`, `ingresos`, `score`).

### Paso 2: Evaluación por fila
Se calcula el porcentaje de valores faltantes por fila y se toma una decisión:

- **Si > 45% → ❌ Eliminación**  
  El registro no aporta suficiente información.

- **Si 21–44% → 🔍 Evaluación**  
  Si al menos el **60% de las columnas críticas están completas**, se imputan solo esas columnas faltantes.  
  Si no, se descarta la fila.

- **Si 1–20% → ✅ Imputación**  
  - Si el dataset tiene ≤ 5000 filas → **KNNImputer**
  - Si tiene > 5000 filas → **IterativeImputer**

### Paso 3: Ejecución de imputación
Solo se imputan las columnas definidas como críticas, preservando el resto del dataset.

### Paso 4: Registro de cambios
Se guardan:
- Las filas eliminadas
- Las filas imputadas
- Un resumen del flujo ejecutado

---

## 🧪 Ejemplo de uso

```python
from ImputacionProceso.missing_processor import MissingProcessor
from ImputacionProceso.report_printer import MissingReportPrinter

# Definir columnas críticas
columnas_criticas = ['edad', 'ingresos', 'score', 'saldo']

# Iniciar procesamiento
processor = MissingProcessor(
    df_missings,
    columnas_relevantes=columnas_criticas,
    columnas_criticas=columnas_criticas,
    knn_neighbors=5
)

# Ejecutar flujo completo
resultado = processor.ejecutar_flujo()

# Imprimir resumen
printer = MissingReportPrinter(processor)
printer.imprimir_resumen()