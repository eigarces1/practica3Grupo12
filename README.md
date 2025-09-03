
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



# 🧹 Limpieza y Transformación de Datos

El dataset original **Global Cybersecurity Threats (2015–2024)** contenía inconsistencias y datos incompletos que requerían preparación antes del análisis. El proceso aplicado en el notebook siguió estas etapas:

---

### 1) Carga e inspección inicial
**Objetivo:** Asegurar la disponibilidad del archivo y una primera inspección de su estructura.
- Descarga del dataset desde Kaggle mediante la API.
- Lectura del archivo `.csv` en un DataFrame de `pandas`.
- Inspección de la estructura (`df.info()`, `df.head()`) y dimensiones (`df.shape`).

---

### 2) Identificación de problemas en los datos

**Objetivo:** Entender calidad, completitud y dispersión de categorías antes de limpiar.

- **Duplicados**: Detección con `df.duplicated().sum()`.  
- **Valores nulos**: Revisión con `df.isnull().sum()`.  
- **Tipos de datos incorrectos**: Algunas columnas numéricas estaban como `object`.  
- **Categorías inconsistentes**: Valores con mayúsculas/minúsculas o espacios adicionales.  

---

### 3) Eliminación de duplicados

**Objetivo:** Evitar el confunsiones que generan registros repetidos en métricas, agregaciones y modelos.
Se eliminaron filas repetidas para evitar sesgo en el análisis.

```python
df = df.drop_duplicates()

---
### 4) Manejo de valores faltantes

- **Numéricos**: Se reemplazaron los `NaN` por `0` en las siguientes columnas:
    - `Financial Loss (in Million $)`
    - `Number of Affected Users`
    - `Incident Resolution Time (in Hours)`
- **Categóricos**: Se rellenaron los valores nulos con `'Unknown'` para mantener la consistencia.

**Ejemplo aplicado en el notebook:**
```python
df['Financial Loss (in Million $)'].fillna(0, inplace=True)
df['Number of Affected Users'].fillna(0, inplace=True)
df['Incident Resolution Time (in Hours)'].fillna(0, inplace=True)
df[categorical_cols] = df[categorical_cols].fillna('Unknown')

---
### 5) Normalización de texto en categorías

Se limpiaron los strings para evitar duplicidad artificial y estandarizar las entradas.

```python
df[col] = df[col].str.strip().str.title()

Esto permitio que entradas como "phishing", "Phishing " y "PHISHING" queden como un único valor "Phishing".

---
### 6) Asegurar tipos de datos correctos

**Objetivo:** Un tipado consistente es crucial para realizar cálculos precisos, ordenamientos correctos y operaciones de series de tiempo de manera eficiente.
Se realizaron conversiones de tipo para garantizar la consistencia en los datos:
- `Year` → `int`
- `Financial Loss (in Million $)` → `float`
- `Number of Affected Users` → `int`
- `Incident Resolution Time (in Hours)` → `int`

---
### 7) Variables derivadas (Feature Engineering)

**Objetivo:** Estas variables permiten un análisis detallado del costo o impacto unitario y una segmentación rápida de los incidentes según su criticidad.
Se crearon nuevas variables para enriquecer el conjunto de datos y permitir análisis más profundos.

#### Impacto económico por usuario afectado
- **Fórmula**: `Impact Per User ($) = (Financial Loss (in Million $) * 1,000,000) / Number of Affected Users`
- **Condición**: Se añade una condición para evitar la división por cero: si `Number of Affected Users` es `0`, el resultado es `0`.

#### Marcador de criticidad
- **Regla**: `IsCritical = 1` si `Incident Resolution Time (in Hours) > 100`, de lo contrario, `0`.
- **Nota**: Este es un umbral ilustrativo que permite segmentar incidentes con alta complejidad o un tiempo de respuesta prolongado.

---
### 8) Listo para análisis y visualización
Con el conjunto de datos depurado y enriquecido, se puede proceder al análisis. Algunos ejemplos de los análisis realizados incluyen:

- **Conteo y tendencias** (por ejemplo, ataques por año).
- **Análisis de distribuciones** por tipo de ataque.
- **Creación de visualizaciones geográficas**, como un **choropleth** con Plotly, para mostrar las pérdidas económicas por país y su evolución temporal.

