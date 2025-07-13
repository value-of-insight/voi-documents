---
sidebar_position: 3
---

# 🧹 Módulo de Corrección de Escritura de Datos

Este módulo forma parte del flujo integral de *data enhancement* y tiene como objetivo principal **detectar y corregir errores en la escritura de datos**, especialmente en columnas categóricas y numéricas que han sido ingresadas de forma inconsistente.

---

## 🎯 Objetivo general

Detectar y corregir automáticamente errores comunes en la escritura de datos, como:

- Errores ortográficos en categorías (`"fememino"` → `"femenino"`)
- Valores mal escritos en números (`"1.500,00"` → `1500.00`)
- Variaciones de texto (`"MASCULINO"` → `"masculino"`)
- Valores vacíos o inválidos

---

## 🔍 Funcionalidades clave

- **Normalización de texto**: minúsculas, eliminación de espacios extra
- **Corrección inteligente**: usa similitud (`fuzzywuzzy`) para encontrar la opción más cercana a partir de listas blancas
- **Limpieza de valores numéricos**: detecta patrones comunes mal formateados y los convierte a `float`
- **Registro de cambios**: muestra qué filas se corrigieron y cuánto cambió cada columna

---

## 📊 Tipos de corrección

| Tipo | Descripción |
|------|-------------|
| **Texto categórico** | Se corrige usando listas blancas y similitud |
| **Valores numéricos** | Se limpian errores frecuentes y se convierten a float |

---
## 📁 Estructura del Paquete

1. **`text_normalizer.py`**: Clase principal: TextNormalizer 
2. **`report_printer.py`**: Genera informe de cambios
3. **`rule_engine.py`**: Opcional: para reglas personalizadas
4. **`error_simulator.py`**: (opcional) Simular errores para pruebas 

---
## 🧩 Flujo del módulo

### Paso 1: Definición de las columnas que serán procesadas por el módulo

El usuario define cuáles son las columnas que deben ser procesadas por el módulo. Estas pueden ser:

- Columnas categóricas (ej: genero, ubi_geografica, estado_civil)
- Columnas numéricas que podrían estar mal escritas (ej: ingresos, score)

```python
LISTAS_BLANCAS = {
    'genero': ['femenino', 'masculino'],
    'ubi_geografica': ['urbano', 'rural', 'extranjero'],
    'preferencia_canal': ['banca_app', 'banca_física', 'banca_linea']
}
```
### Paso 2: Normalización y corrección de texto
Se recorren las columnas definidas como relevantes y se aplica:
- Para columnas **categóricas :**
  - Se convierte todo a minúsculas y sin espacios extra
  - Se compara cada valor contra una lista blanca
  - Si hay similitud alta (>80% con fuzzywuzzy), se corrige al valor válido más cercano
  - Si no hay coincidencia clara, se marca como NaN
  
- Para columnas **numéricas :**
  - Se eliminan caracteres especiales (ej: letras o símbolos en números)
  - Se corrigen formatos comunes ("1,500.34" → "1500.34")
  - Se convierten a float o int según corresponda

### Paso 3: Registro de cambios realizados
Se genera un registro (log) para mostrar qué filas fueron corregidas y qué columna tuvo cambios.

**Visualización del log 👀**

| Columnas | Cambios Realizados |
|----------|--------------------|
| `genero` | 3|
| `ingresos`| 2|
| `score` | 5|

### Paso 4: Resultado final
El módulo devuelve:
- Un DataFrame con las columnas corregidas
- Un registro detallado de los cambios
- Opcionalmente, columnas nuevas con sufijo _corregido para comparar antes/después

---
## 🧪 Ejemplo de uso
---

```python
from CorreccionEscritura.text_normalizer import TextNormalizer
from CorreccionEscritura.report_printer import WritingReportPrinter
from CorreccionEscritura.error_simulator import introducir_errores_escritura

# Cargar datos
ruta = r'C:\Users\Joyssie\Downloads\data.csv'
df = pd.read_csv(ruta)

# Simular errores (opcional, para testing)
columnas_categoricas = list(LISTAS_BLANCAS.keys())
df_con_errores = introducir_errores_escritura(df, columnas_categoricas)

# Iniciar proceso de corrección
normalizer = TextNormalizer(df_con_errores)
df_corregido = normalizer.aplicar_correcciones()

# Imprimir resultados
printer = WritingReportPrinter(normalizer)
printer.imprimir_resumen()

```