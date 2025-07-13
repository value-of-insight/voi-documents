---
sidebar_position: 1
---

# 🔍 Módulo de Detección y Manejo de Duplicados

Este módulo forma parte del flujo integral de *data enhancement* y tiene como objetivo principal **detectar y resolver duplicados en un conjunto de datos**, priorizando la conservación del registro más representativo por cliente.

> ⚠️ Este módulo debe ejecutarse **después de imputar missings, corregir tipos de dato y normalizar texto**, ya que esos pasos son fundamentales para evitar falsos positivos o errores en la comparación de registros.

---

## 🎯 Objetivo general

Detectar clientes con múltiples registros (duplicados) y elegir **el más representativo** según su perfil numérico o categórico. Esto permite:

- Mejor calidad de datos para modelos predictivos
- Reducir sesgos causados por datos redundantes
- Mantener solo una fila por cliente sin perder información relevante

---

## 🧠 Funcionalidades clave

| Característica | Descripción |
|----------------|-------------|
| **Detección automática** | Identifica filas duplicadas basándose en una columna clave (`ID_Cliente`, `dni`, etc.) |
| **Decisión inteligente** | Selecciona el mejor registro usando distancia euclidiana (si grupo es categórico) o proximidad al promedio (si grupo es numérico) |
| **Registro claro** | Genera un informe con estadísticas de duplicados encontrados y eliminados |
| **Modularidad** | Fácil de integrar con otros módulos del flujo |

---
## 📁 Estructura del Paquete

1. **`duplicate_detector.py`**: Clase principal - DuplicateDetector
2. **`duplicate_reporter.py`**: Generador de informes

---

## 🧩 Flujo del módulo

### Paso 1: Cargar DataFrame ya limpio
El dataset debe haber pasado por:
- Imputación de missings
- Corrección de tipo de dato
- Normalización de texto

### Paso 2: Definir columnas clave

- `id_col`: identificador único del cliente (ej: `ID_Cliente`)
- `grupo_col`: variable usada para evaluar el perfil del cliente (ej: `Distrito_Residencia`, `Edad`)
- `variables_numericas`: columnas usadas para comparar perfiles (ej: `Ingresos`, `Score`, `Saldo`)

Ejemplo:

```python
id_cliente = 'ID_Cliente'
grupo_seleccion = 'Distrito_Residencia'
variables_numericas = ['Edad', 'Ingresos', 'Score', 'Saldo']
```

### Paso 3: Detectar duplicados por cliente
Se agrupa por id_col y se buscan clientes con más de un registro.

### Paso 4: Resolver qué fila conservar
- Si grupo_col es categórico → se compara cada fila contra el perfil promedio del grupo
- Si grupo_col es numérico → se elige el registro más cercano al promedio general

### Paso 5: Registrar cambios realizados
Se genera un resumen con:
- Total de registros procesados
- Clientes únicos
- Número de duplicados encontrados y eliminados
- Ejemplo de datos filtrados

---
## 🧪 Ejemplo de uso
---

```python
from DeteccionDuplicados.duplicate_detector import DuplicateDetector
from DeteccionDuplicados.duplicate_reporter import DuplicateReporter

# Cargar datos
ruta = r'C:\Users\Joyssie\Downloads\data.csv'
df = pd.read_csv(ruta)

# Definir columnas clave
id_cliente = 'ID_Cliente'
grupo_seleccion = 'Distrito_Residencia'
variables_numericas = ['Edad', 'Ingresos', 'Score', 'Saldo']

# Iniciar proceso de detección
detector = DuplicateDetector(df, id_cliente, grupo_seleccion, variables_numericas)
df_limpio, resumen = detector.detectar_y_manejar_duplicados()

# Imprimir resultados
reporter = DuplicateReporter(detector)
reporter.imprimir_resumen()

```