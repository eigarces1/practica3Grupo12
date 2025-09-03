
# 📊 Clase: Tratamiento de Datos  

**Grupo #12**


### 👥 Integrantes  
- Martha Aguilar  
- Elian Garces  

---

﻿**Práctica #3**
 
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
**Objetivo:** Asegurar la disponibilidad del archivo y una primera inspección de su estructura.
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

<img width="837" height="201" alt="image" src="https://github.com/user-attachments/assets/c3683e3a-7784-45d6-8eae-f164b94b352b" />

---

### 5) Normalización de texto (categorías)

**Objetivo:** Homogeneizar las entradas "phishing", "Phishing" y "PHISHING" en un único valor: "Phishing".

- Limpieza y homogeneización de categorías:
  - `str.strip()` → elimina espacios en blanco sobrantes.
  - `str.title()` → estandariza capitalización (e.g., `phishing` → `Phishing`).

 `df[col] = df[col].str.strip().str.title()`.

---

### 6) Asegurar tipos de datos correctos

**Objetivo:** Asegurar un tipado consistente para cálculos, ordenamientos y operaciones con series temporales.
- `Year` → `int`
- `Financial Loss (in Million $)` → `float`
- `Number of Affected Users` → `int`
- `Incident Resolution Time (in Hours)` → `int`

---

### 7) Variables derivadas (feature engineering)

**Objetivo:** Habilitar análisis de costo/impacto unitario y segmentación rápida de incidentes.

- **Impacto económico por usuario afectado**:  
  `Impact Per User ($) = (Financial Loss (in Million $) * 1_000_000) / Number of Affected Users`  
  (si `Number of Affected Users == 0` → `0` para evitar división por cero).

- **Marcador de criticidad**:  
  `IsCritical = 1` si `Incident Resolution Time (in Hours) > 100`, en caso contrario `0`.  
  *(Umbral ilustrativo para segmentar incidentes con alta complejidad/tiempo de respuesta).*
  
  <img width="1265" height="391" alt="Captura" src="https://github.com/user-attachments/assets/161a360f-96f4-4aef-91c8-4f83b21f386f" />

---

### 8) Listo para análisis/visualización
Con el dataset depurado y enriquecido:
- Se realizan conteos y tendencias (p. ej., ataques por año).
- Se analizan distribuciones por tipo de ataque.
- Se construyen visualizaciones geográficas (ej. **choropleth** con Plotly) sobre pérdidas económicas por país y su evolución temporal.

---
## 🔍 Principales Hallazgos del Análisis

El análisis del conjunto de datos **Global Cybersecurity Threats (2015–2024)** reveló varias tendencias clave en el panorama de la ciberseguridad. A través de la limpieza y visualización de los datos, identificamos los tipos de ataque más comunes, los sectores industriales más vulnerables y el impacto geográfico y económico de estos incidentes.

A continuación, se exponen los principales hallazgos, contextualizados con referencias de investigaciones y reportes de la industria de los últimos años.

---

### 1️⃣ Tipos de Ataque

El análisis de los datos revela que los ciberataques más frecuentes durante el período son **Phishing**, **DDoS** y **SQL Injection**. A continuación, se detalla en qué consiste cada uno:

- **Phishing**: Una técnica de ciberataque que utiliza el engaño para robar información personal y confidencial. Los atacantes se hacen pasar por una entidad legítima (como un banco o una red social) para persuadir a la víctima a que revele datos sensibles como contraseñas, números de tarjetas de crédito o credenciales de acceso.

- **DDoS (Ataque de Denegación de Servicio Distribuido)**: Un ataque DDoS busca saturar un servidor, servicio o red con un tráfico masivo de peticiones desde múltiples fuentes. Su objetivo principal no es robar datos, sino hacer que el servicio deje de estar disponible para sus usuarios legítimos.

- **SQL Injection (Inyección SQL)**: Una técnica de ataque a bases de datos en la que los atacantes inyectan código SQL malicioso a través de campos de entrada de un sitio web (como formularios de inicio de sesión o barras de búsqueda) para manipular la base de datos subyacente.

Los hallazgos coinciden con las tendencias globales reportadas por múltiples organizaciones. Informes del Parlamento Europeo (2025) confirman que el phishing es una de las principales amenazas. Es importante considerar que otros ciberataques han evolucionado de manera significativa, como el ransomware, que ha pasado de ser un ataque que encripta datos a un modelo de extorsión de doble impacto (Positive Technologies, 2025). Además, un análisis de ResearchGate (2024) señala que el uso de malware ha sido una de las principales metodologías de ataque, responsable de hasta el 66% de los ataques exitosos contra organizaciones.

*A continuación, un gráfico de barras visualiza los tipos de ataques más frecuentes.*

<img width="925" height="462" alt="tipo ataque" src="https://github.com/user-attachments/assets/9ef15217-a994-4009-a56f-d9e243db3cd0" />

---

### 2️⃣ Sectores Industriales más Afectados

Los datos analizados muestran que las industrias más atacadas son las de **Tecnología de la Información (TI)**, la **Bancaria** y la de **Salud**.

- El sector de TI es un objetivo principal debido a su papel central en la infraestructura digital y el manejo de datos, según un análisis de Khairat (2025).

- El sector financiero es un blanco recurrente para los ciberataques debido a su gran volumen de transacciones y datos sensibles. Un informe del FMI (2021) y otros estudios confirman que el sector financiero es uno de los más atacados por la sofisticación y el potencial de ganancias.

- El sector de la salud también es un objetivo frecuente de ransomware debido al valor de los datos médicos y la urgencia de reanudar operaciones (Everbridge, s. f.), lo que lo hace particularmente vulnerable.

*A continuación se muestra un gráfico de barras que evidencia que la industria de TI es la más atacada.*

<img width="969" height="472" alt="industrias" src="https://github.com/user-attachments/assets/89a66842-c88c-4b8a-9e7a-b59f7b4846e5" />

---

### 3️⃣ Consecuencias Económicas de los Ciberataques

Gracias al análisis realizado, se puede destacar que **Alemania** es el país más afectado económicamente por los ciberataques. Este hallazgo se puede contextualizar con estudios más amplios sobre el impacto financiero.

- Según un informe de la asociación de la industria alemana Bitkom, los daños anuales por robo de datos, espionaje industrial y sabotaje ascendieron a **266,6 mil millones de euros en 2024**, un aumento del 29% con respecto al año anterior (Agenzia Nova, 2024).

- Estos ataques tienen como objetivo principal a la economía alemana, con un enfoque en la industria manufacturera y la tecnología.

- Un informe del Banco Mundial (2024) diferencia los **costos directos** (como pagos de rescate) de los **costos indirectos** (pérdida de valor de mercado, daño a la reputación e interrupción de la cadena de suministro).

- Para ilustrar este impacto, un estudio de Investopedia (s. f.) encontró que las empresas afectadas por una brecha de datos experimentan una **caída de casi el 3,5% en el precio de sus acciones**, y los sectores de la salud, el financiero y el manufacturero sufren las mayores caídas.

- Un ejemplo concreto del impacto devastador se observa en el caso de una empresa alemana centenaria que se vio obligada a cerrar permanentemente después de que un ciberataque paralizara por completo sus operaciones y su capacidad para generar ingresos (Bitlife Media, 2025).

- Finalmente, un análisis del CEPR (2025) señala que el **costo promedio global de una filtración de datos alcanzó los 4,88 millones de dólares en 2024**.

*A continuación se muestra un gráfico que evidencia que el país mas afectado es Alemania.*

<img width="1212" height="408" alt="impacto eco" src="https://github.com/user-attachments/assets/12a84c3f-f5d5-480c-ad5f-4210785342de" />

**Mapa mundial de perdidas económicas por ciberataques**
<img width="1194" height="444" alt="mapa" src="https://github.com/user-attachments/assets/a9a7b82b-874a-41ae-9229-988a3f182378" />


---

### 4️⃣ Relación entre Usuarios Afectados y Pérdidas Económicas

El gráfico de dispersión revela que **no existe una correlación directa y uniforme** entre el número de usuarios afectados y las pérdidas económicas causadas por los ciberataques. De hecho:

- **Alta dispersión de resultados**: Hay casos donde muchos usuarios están comprometidos, pero las pérdidas son moderadas. Por el contrario, un número reducido de afectados puede generar consecuencias financieras muy elevadas.

- **Importancia del tipo de ataque**: Al distinguir por tipo de ataque, se observa que amenazas como ransomware o DDoS suelen producir impactos económicos altos, incluso con pocos usuarios afectados. En cambio, ataques más comunes como phishing o malware tienden a generar menos pérdidas de forma masiva.

📚 *Este hallazgo está respaldado por literatura especializada:*

- Shevchenko et al. (2022) encontraron que no se identifica una relación consistente entre el número de registros afectados y la magnitud del daño. Los eventos graves tienden a seguir distribuciones con colas pesadas, lo que sugiere que unos pocos incidentes generan pérdidas extraordinariamente altas.

- El FMI también señala que, aunque la mayoría de las pérdidas directas por incidentes cibernéticos son modestas, algunos eventos extremos pueden alcanzar cientos de millones de dólares, representando amenazas serias para la liquidez o solvencia de las empresas.

- Además, estudios como el de CybelAngel indican que en ataques tipo ransomware, los **costos promedio de recuperación —sin incluir el rescate— pueden alcanzar 1.82 M USD**, y los rescates en sí mismos suman otros millones, sin garantía de restauración completa de datos.

<img width="994" height="448" alt="grafico de dispersion" src="https://github.com/user-attachments/assets/da4a368a-b468-4005-8d9a-d3a9a22fd506" />

---

### 📂 Conclusiones del proyecto

El análisis exploratorio (EDA) realizado sobre el conjunto de datos de amenazas cibernéticas ha demostrado que la preparación y limpieza de datos es el primer paso y el más crucial para obtener *insights* significativos. Aunque los datos originales podían parecer confusos, la organización y visualización de la información permitieron revelar tendencias claras y cuantificables en el panorama de la ciberseguridad.

Los hallazgos clave de este análisis refuerzan que las amenazas cibernéticas no son una preocupación abstracta, sino un riesgo tangible con un impacto económico directo y a menudo devastador. La identificación de ataques predominantes como **Phishing**, **DDoS** y **SQL Injection**, y el hecho de que sectores como TI, la banca y la salud sean los más afectados, nos proporciona una hoja de ruta clara para la toma de decisiones estratégicas.

Quizás el *insight* más importante es la falta de una correlación directa entre el número de usuarios afectados y las pérdidas económicas. Esto subraya que el valor de los datos comprometidos es más importante que la cantidad de personas afectadas. Para las empresas, la protección de información crítica y la implementación de defensas contra ataques de alto impacto, como el *ransomware*, son más cruciales que nunca.

En resumen, este trabajo demuestra que el análisis de datos no solo permite entender el problema, sino que también ofrece un fundamento sólido para la creación de estrategias de ciberseguridad más efectivas y dirigidas.

---

### 📂 Referencias

* [**Global Cybersecurity Threats (2015-2024)**](https://www.kaggle.com/datasets/ahmedaliabdulrahman/global-cybersecurity-threats)
* **European Parliament.** (2025, 26 de junio). Cybersecurity: main and emerging threats. Recuperado de https://www.europarl.europa.eu/topics/en/article/20220120STO21428/cybersecurity-main-and-emerging-threats
* **Positive Technologies.** (2025). Cybersecurity threatscape: Q4 2024 – Q1 2025. Recuperado de https://global.ptsecurity.com/en/research/analytics/cybersecurity-threatscape-q4-2024-q1-2025/
* **ResearchGate.** (2024). The Evolution of Cybersecurity Threats and Strategies for Effective Protection. A review. Recuperado de https://www.researchgate.net/publication/385685835_The_Evolution_of_Cybersecurity_Threats_and_Strategies_for_Effective_Protection_A_review
* **Khairat, O.** (2025). Cyber Threat Metrics & Global Impact Analysis (2015–2024). Medium. Recuperado de https://medium.com/@Khairattheanalyst/cyber-threat-metrics-global-impact-analysis-2015-2024-2f47e322eeb2
* **International Monetary Fund (IMF).** (2021). The Global Cyber Threat to Financial Systems. Finance & Development. Recuperado de https://www.imf.org/external/pubs/ft/fandd/2021/03/global-cyber-threat-to-financial-systems-maurer.htm
* **Everbridge.** (s. f.). The impact of cybersecurity risks on financial services. Recuperado de https://www.everbridge.com/blog/the-impact-of-cybersecurity-risks-on-financial-services/
* **World Bank.** (2024). A Review of the Economic Costs of Cyber Incidents. Recuperado de https://documents1.worldbank.org/curated/en/099092324164536687/pdf/P17876919ffee4079180e81701969ad0a18.pdf
* **Investopedia.** (s. f.). 10 Ways Cybercrime Impacts Business. Recuperado de https://www.investopedia.com/financial-edge/0112/3-ways-cyber-crime-impacts-business.aspx
* **CEPR.** (2025). Cybersecurity vulnerabilities and their financial impact. VoxEU.org. Recuperado de https://cepr.org/voxeu/columns/cybersecurity-vulnerabilities-and-their-financial-impact
* **Agenzia Nova.** (2024, 28 de agosto). Alemania: Bitkom y los ciberataques a la economía alemana aumentan. Recuperado de https://www.agenzianova.com/es/news/germania-bitkom-aumentano-i-cyber-attacchi-alleconomia-tedesca/
* **Bitlife Media.** (2025, 23 de junio). Una empresa centenaria alemana echa el cierre en cuestión de horas por un ciberataque. Recuperado de https://bitlifemedia.com/2025/06/empresa-centenaria-alemana-cierra-ciberataque/











