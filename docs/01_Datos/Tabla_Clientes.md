# 🧪 Generación de Datos Sintéticos de Clientes con Matriz de Covarianza

Este script permite generar un dataset sintético realista de clientes utilizando una matriz de covarianzas personalizada. Se simulan características sociodemográficas, financieras, de comportamiento bancario y preferencias de canal, útiles para pruebas de modelos, análisis exploratorio, imputación o segmentación.

---

## 📦 Librerías Utilizadas

```python
import numpy as np
import pandas as pd
import statsmodels.api as sm
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import MinMaxScaler
import warnings
import random
from datetime import datetime, timedelta
```

---
##  Paso 1: Definicion de Variables 📃

Se define un conjunto de 16 variables principales con su media 0 (para la simulación) y una matriz de covarianza que modela las relaciones entre ellas.

```python
variables = ["Edad", "Ubi_Geográfica", "Género","Estado_Civil", "Tipo_Trabajo", "Tran_CuentasAhorro", "Tran_TarjetaCredito", "Comp_Pago", "Cant_CuentasAhorro", "Cant_TarjetasCrédito", "Cant_PréstamosVigentes", "Monto_CuentasAhorro","Monto_LíneaCrédito", "Monto_PréstamosVigente" "Preferencia_Canal", "Antigüedad_Suscripción"]
```

---
## Paso 2: Generación de ID'S 🆔
Se generan IDs únicos para clientes
- ID_Cliente: Código único con el prefijo CL.
```python
ID_Clientes = [f"CL{str(i).zfill(6)}" for i in range(1, 100001)]
```
---

## Paso 2: Simulación con Correlaciones 🧮 
Se genera una muestra de **100,000** observaciones correlacionadas usando `np.random.multivariate_normal`. Luego se normalizan usando MinMaxScaler y se ajustan los rangos de valores para que se asemejen a datos reales.

Variables como `Edad`, `Monto_PréstamosVigente`, `Tran_TarjetaCredito`, etc., se transforman con escalas específicas para darle sentido práctico.

---

##  Paso 3: Transformación de Variables Categóricas 🧑‍💼

Se convierten varias columnas numéricas en variables categóricas realistas:

- `Género`: Masculino / Femenino
- `Tipo_Trabajo`: Dependiente / Independiente
- `Comp_Pago`: Puntual / Medio / Moroso (basado en percentiles)
- `Ubi_Geográfica`: Extranjero / Urbano / Rural
- `Preferencia_Canal`: Banca Física / App / En Línea
- `Estado_Civil`: Viudo / Soltero / Casado

---
## Paso 4: Asignación Geográfica 🗺️
Se asignan distritos reales de **Lima Metropolitana** a los clientes con residencia urbana, y se categoriza el tipo de distrito según su zona geográfica. Esta clasificación es útil para análisis sociodemográficos, segmentación de mercado y estrategias geográficas.

| Categoría        | Distritos Incluidos                                                                                                                                  |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Lima Norte 👆🏼**   | Ancón, Carabayllo, Comas, Independencia, Los Olivos, Puente Piedra, San Martín de Porres, Santa Rosa                                               |
| **Lima Top 🏢**     | San Isidro, La Molina, San Borja, Miraflores, Surco, Barranco                                                                                       |
| **Lima Este 👉🏼**    | Ate, Chaclacayo, Lurigancho, La Victoria, Rímac, Chorrillos, San Juan de Lurigancho, San Luis, Cieneguilla, Santa Anita                             |
| **Lima Moderna ✨** | Jesús María, Surquillo, Lince, Magdalena del Mar, San Miguel, Pueblo Libre                                                                          |
| **Ninguno 💨**      | Otros distritos no categorizados explícitamente en el script                                                                                        |

> ℹ️ *Los distritos no clasificados en las cuatro primeras categorías son asignados automáticamente como `"Ninguno"`.*


---

## Paso 5:  Clasificación del Tipo de Cliente 🧠
Se define una función personalizada para categorizar a los clientes en 5 segmentos, según reglas de negocio basadas en edad, productos financieros, canal bancario, comportamiento de pago, entre otros criterios:

| Segmento                 |Condiciones                                                                                                          |
|--------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Cliente Premium ⭐**       | - Edad > 30  <br> - Tarjetas de crédito > 2  <br> - Cuentas de ahorro > 2  <br> - Monto en cuentas > 10,000 <br> - Tipo de trabajo: Independiente <br> - Distrito: Lima Top <br> - Comp. de pago: Puntual |
| **Cliente Tradicional 👩🏼‍🦳**   | - Edad > 60  <br> - Tarjetas de crédito > 2  <br> - Cuentas de ahorro > 2  <br> - Monto en cuentas > 6,000 <br> - Tipo de trabajo: Dependiente o Independiente <br> - Preferencia canal: Banca_Física |
| **Cliente Iniciador ⏳**     | - Edad > 18  <br> - ≥1 tarjeta de crédito <br> - ≥1 cuenta de ahorro <br> - Monto en cuentas > 2,000 <br> - Tipo de trabajo: Dependiente <br> - Preferencia canal: Banca_App <br> - Ubicación: Urbano |
| **Cliente Joven Profesional 👨🏻‍💻** | - Edad entre 26 y 34 <br> - Tarjetas de crédito > 2 <br> - Cuentas de ahorro > 2 <br> - Monto en cuentas > 10,000 <br> - Tipo de trabajo: Dependiente o Independiente <br> - Canal: Banca_App o Banca_Linea |
| **Cliente Regular 👨🏻**       | No cumple con las condiciones anteriores|

---

## Resultado : Dataset Final 📚
El resultado es un DataFrame con más de 100,000 registros y múltiples columnas simuladas con lógica de negocio, incluyendo:

- Información financiera (montos, cuentas, tarjetas)
- Información demográfica (edad, género, estado civil)
- Comportamiento (comp_pago, canal preferido)
- Ubicación geográfica y categorización
- Segmentación de clientes 