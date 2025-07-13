---
sidebar_position: 1
---

# 🧰 Módulo de Corrección de Consistencia de Tipo de Dato

Este módulo forma parte del proceso integral de *data enhancement* y tiene como objetivo principal **detectar y corregir inconsistencias en los tipos de datos** presentes en un conjunto de datos.  

Se enfoca en convertir las columnas al tipo más adecuado (entero, float, fecha o cadena), registrando cada cambio realizado y generando informes claros del proceso.

---
## 🎯 Objetivo general

Detectar automáticamente el **tipo de dato dominante** en cada columna y **corregir valores inconsistentes** para que se ajusten a ese tipo. Esto permite:

- Mejor rendimiento en modelos predictivos
- Mayor consistencia en análisis exploratorios
- Preparar el dataset para pasos posteriores (como normalización de texto o detección de duplicados)

---

## 🔍 Funcionalidades clave

- Detecta automáticamente el tipo de dato dominante en cada columna:
  - `int`: número entero
  - `float`: número decimal
  - `datetime`: fechas
  - `str`: texto o categórico
- Convierte la columna al tipo detectado
- Genera un **registro (log)** de los cambios realizados por columna
- Permite definir manualmente qué columnas deben tratarse como fechas
- Opcional: introduce errores simulados para pruebas

---

## 📁 Estructura del paquete

1. **`error_simulator.py`**: Simular errores (opcional) 
2. **`consistency_checker.py`**: Detectar tipo dominante
3. **`type_fixer.py`**: Clase principal para corrección
4. **`report_printer.py`**: Generador de informe

---

## 🧩 Flujo del módulo
## Paso 1: Cargar DataFrame original
El usuario carga el dataset ya limpio del paso anterior (por ejemplo, después de haber aplicado imputación de missings).


## Paso 2: (Opcional) Introducir errores 
Se puede simular errores en ciertas columnas para probar el flujo completo del módulo. Este paso es opcional y solo se usa durante desarrollo o testing. Por ejemplo: `F3m` en lugar de `Femenino` o `doscientos` en vez de `200`


## Paso 3: Detectar tipo dominante por columna
Para cada columna del dataset, el algoritmo detecta automáticamente el tipo de dato más probable , basándose en la mayoría de los valores presentes.

## 📊 Tipos soportados

| Tipo | Descripción |
|------|-------------|
| `int`     | Valores numéricos enteros |
| `float`   | Números con decimales |
| `datetime`| Fechas y horas |
| `str`     | Cadenas de texto y categorías |

### Lógica de detección:
- Si hay fechas reconocibles y superan un umbral → se marca como datetime
- Si hay mayoritariamente números → decide entre int y float según proporción
- Si hay mezcla de formatos pero no suficiente como para inferir número o fecha → se considera str

## Paso 4: Aplicar corrección según tipo detectado
Una vez identificado el tipo dominante, se aplica la corrección adecuada:

| Tipo Detectado | Acción Realizada |
|------|-------------|
| `int`     | Se convierte a entero. Valores no convertibles → `Nan`|
| `float`   | Se convierte a decimal|
| `datetime`| Se intenta parsear como fecha. No válido → `NaN`|
| `str`     | Se convierte a cadena limpia (strip, minúsculas, etc.)|

## Paso 5: Registrar cambios en LOG
Se genera un registro (log) con información clave:

- Columna procesada
- Tipo original
- Tipo detectado
- Número de cambios realizados

**Visualización de log**
| Columna | Tipo Original |Tipo Detectado|Cambios Realizados|
|------|-------------|---------------------|----------------|
| `edad`|`object`| `int`|2|
| `score`|`object`| `float`|1|
| `fecha_solicitud`|`object`|`datetime`|1|


## Paso 6: Mostrar informe final
Se muestra un informe claro del proceso, incluyendo:
- Resumen de columnas corregidas
- Total de cambios realizados
- Ejemplo de datos antes/después
  
---

## 🧪 Ejemplo de uso

```python
from ConsistenciaDatos.type_fixer import TypeFixer
from ConsistenciaDatos.report_printer import ConsistencyReportPrinter
from ConsistenciaDatos.error_simulator import introducir_errores

# Cargar datos
ruta = r'C:\Users\Joyssie\Downloads\data.csv'
df = pd.read_csv(ruta)

# Simular errores (opcional)
df_con_errores = introducir_errores(df)

# Iniciar proceso de corrección
fixer = TypeFixer(df_con_errores)
df_corregido = fixer.limpiar_tipos()

# Imprimir resumen del proceso
printer = ConsistencyReportPrinter(fixer)
printer.imprimir_resumen()
