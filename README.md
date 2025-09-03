
# 📊 Clase: Tratamiento de Datos  
**Grupo #12**


### 👥 Integrantes  
- Martha Aguilar  
- Elian Garces  

---

﻿# **Practica #3**
 
# 🛡️ Informe sobre el Dataset *Global Cybersecurity Threats (2015–2024)*  

## 📌 Propósito del Dataset  
El presente informe se basa en el dataset **[Global Cybersecurity Threats (2015–2024)](https://www.kaggle.com/datasets/atharvasoundankar/global-cybersecurity-threats-2015-2024)** disponible en *Kaggle*.  

Nuestro grupo eligió este conjunto de datos porque constituye una excelente fuente para el análisis de ciberseguridad, ya que:  

- Dispone de una base robusta con múltiples variables relevantes.  
- Refleja cómo los ataques cibernéticos han impactado a diferentes industrias a nivel global.  
- Permite realizar tanto análisis estadísticos como explicaciones visuales.  
- Se presta para procesos de **limpieza de datos, análisis exploratorio (EDA), visualización y modelado predictivo**.  

En resumen, este dataset resulta ideal para combinar técnicas de tratamiento de datos con aplicaciones prácticas en seguridad informática.  

---

## 📂 Descripción del Dataset  
Este conjunto de datos ofrece una visión integral de las **amenazas cibernéticas globales** entre 2015 y 2024. Contiene información detallada sobre:  

- 🔸 **Tipos de ataques** (phishing, ransomware, DDoS, etc.)  
- 🔸 **Industrias objetivo** (salud, educación, finanzas, tecnología, etc.)  
- 🔸 **Países afectados**  
- 🔸 **Pérdidas financieras estimadas**  
- 🔸 **Número de usuarios afectados**  
- 🔸 **Tiempo de resolución de incidentes**  
- 🔸 **Mecanismos de defensa utilizados** (firewalls, IA, VPN, etc.)  
- 🔸 **Vulnerabilidades explotadas** (contraseñas débiles, ingeniería social, zero-day, etc.)  
- 🔸 **Fuentes de ataque** (grupos de hackers, insiders, estados-nación, etc.)  


## 🧹 Limpieza y Transformación de Datos

A continuación se detallan las etapas de preparación del dataset **Global Cybersecurity Threats (2015–2024)** que se implementan en el notebook.

### 1) Ingesta y configuración inicial
**Objetivo:** asegurar la disponibilidad del archivo y una primera inspección de su estructura.
- Descarga del dataset desde Kaggle (vía API) y descompresión del archivo.
- Carga en memoria con `pandas.read_csv(...)` (codificación `UTF-8`).
- Verificación básica: `df.shape`, `df.head()`.



---

### 2) Inspección del estado del dataset
**Objetivo:** entender calidad, completitud y dispersión de categorías antes de limpiar.
- **Estructura y tipos**: `df.info()`.
- **Valores faltantes por columna**: `df.isnull().sum()`.
- **Duplicados**: `df.duplicated().sum()`.
- **Exploración de cardinalidad** en variables categóricas relevantes (muestra de valores únicos), como:
  - `Country`, `Year`, `Attack Type`, `Target Industry`,
  - `Financial Loss (in Million $)`, `Number of Affected Users`,
  - `Attack Source`, `Security Vulnerability Type`, `Defense Mechanism Used`,
  - `Incident Resolution Time (in Hours)`.



---

### 3) Eliminación de duplicados


**Objetivo:** Evitar el confunsiones que generan registros repetidos en métricas, agregaciones y modelos.
Se eliminaron filas repetidas para evitar sesgo en el análisis.

- `df = df.drop_duplicates()`.

---

### 4) Tratamiento de valores faltantes (NA)

**Objetivo:** Garantizar operaciones aritméticas y agregaciones sin errores; evitar sesgos de eliminación masiva (`dropna`).
- **Numéricos**:
  - `Financial Loss (in Million $)` → `fillna(0)`
  - `Number of Affected Users` → `fillna(0)`
  - `Incident Resolution Time (in Hours)` → `fillna(0)`
- **Categóricos**:
  - Relleno general con la etiqueta `'Unknown'` para columnas categóricas.

**Ejemplo aplicado en el notebook:**

- `df['Financial Loss (in Million $)'].fillna(0, inplace=True)`.
- `df['Number of Affected Users'].fillna(0, inplace=True)`.
- `df['Incident Resolution Time (in Hours)'].fillna(0, inplace=True)`.
- `df[categorical_cols] = df[categorical_cols].fillna('Unknown')`.

---

### 5) Normalización de texto (categorías)

**Objetivo:** Homogeneizar las entradas "phishing", "Phishing" y "PHISHING" en un único valor: "Phishing".

- Limpieza y homogeneización de categorías:
  - `str.strip()` → elimina espacios en blanco sobrantes.
  - `str.title()` → estandariza capitalización (e.g., `phishing` → `Phishing`).

- `df[col] = df[col].str.strip().str.title()`.


---

### 6) Asegurar tipos de datos correctos
- `Year` → `int`
- `Financial Loss (in Million $)` → `float`
- `Number of Affected Users` → `int`
- `Incident Resolution Time (in Hours)` → `int`

**Motivación:** tipado consistente para cálculos, ordenamientos y operaciones de series de tiempo.

---

### 7) Variables derivadas (feature engineering)
- **Impacto económico por usuario afectado**:  
  `Impact Per User ($) = (Financial Loss (in Million $) * 1_000_000) / Number of Affected Users`  
  (si `Number of Affected Users == 0` → `0` para evitar división por cero).

- **Marcador de criticidad**:  
  `IsCritical = 1` si `Incident Resolution Time (in Hours) > 100`, en caso contrario `0`.  
  *(Umbral ilustrativo para segmentar incidentes con alta complejidad/tiempo de respuesta).*

**Motivación:** habilitar análisis de costo/impacto unitario y segmentación rápida de incidentes.

---

### 8) Listo para análisis/visualización
Con el dataset depurado y enriquecido:
- Se realizan conteos y tendencias (p. ej., ataques por año).
- Se analizan distribuciones por tipo de ataque.
- Se construyen visualizaciones geográficas (ej. **choropleth** con Plotly) sobre pérdidas económicas por país y su evolución temporal.

---



