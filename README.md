Explorando factores de comportamiento en NovaRetail+

Análisis exploratorio y de correlación sobre el comportamiento de 15,000 clientes de NovaRetail+, una plataforma de comercio electrónico con presencia en Latinoamérica, para identificar qué factores se asocian con el ingreso que cada cliente genera y con el abandono de la plataforma.

Proyecto desarrollado como parte del Bootcamp de Análisis de Datos (Sprint 8).

🎯 Objetivo del proyecto

Responder a la pregunta de negocio:

¿Qué factores del comportamiento del cliente están más fuertemente asociados con el ingreso anual generado?

La métrica principal es ingreso_anual, que mide el valor económico que cada cliente aporta a la empresa. De forma complementaria, el análisis explora qué variables se relacionan con el abandono de la plataforma, para orientar estrategias de retención y crecimiento.

📊 Dataset utilizado

Archivo: novaretail_comportamiento_clientes_2024.csv Tamaño: 15,000 registros × 12 columnas, sin valores nulos.

Columna	Tipo	Descripción
id_cliente	Identificador	Identificador único del cliente
edad	Numérica	Edad del cliente (18 a 75 años)
nivel_ingreso	Numérica	Ingreso anual estimado del cliente
visitas_mes	Numérica	Visitas a la app o sitio web en el mes
compras_mes	Numérica	Compras realizadas en el mes
gasto_publicidad_dirigida	Numérica	Gasto en anuncios asignado al usuario
satisfaccion	Numérica	Calificación de satisfacción (escala 1 a 5, con decimales)
miembro_premium	Binaria	1 = tiene suscripción premium, 0 = no
abandono	Binaria	1 = abandonó la plataforma, 0 = no
tipo_dispositivo	Categórica	Móvil, escritorio o tablet
region	Categórica	Norte, sur, este u oeste
ingreso_anual	Numérica	Ingreso anual que el cliente genera para la empresa

Nota: ingreso_anual no es el ingreso personal del cliente (ese es nivel_ingreso), sino lo que el cliente aporta a NovaRetail+.

🛠️ Herramientas
Python (Google Colab)
pandas y NumPy para manipulación de datos
matplotlib y seaborn para visualización
SciPy (pointbiserialr, chi2_contingency) para pruebas estadísticas
🔎 Etapas del análisis
1. Preparación de datos
Revisión de estructura, tipos de datos y valores nulos.
Conversión de edad de decimal a entero, tras verificar que no existían valores fraccionarios reales.
Validación de que las variables binarias estuvieran codificadas como 0/1 y de que las categóricas no requirieran transformación.
2. Exploración de variables
Numéricas: estadística descriptiva y diagnóstico de distribución, sesgo y posibles outliers. Destaca que al menos el 25 % de los clientes no compró en el mes y que ingreso_anual tiene un fuerte sesgo a la derecha.
Binarias: proporción de clientes premium (13.93 %) y tasa general de abandono (15.07 %), además del cruce entre ambas.
Categóricas: distribución por dispositivo (móvil domina con 65.5 %) y por región (distribución equilibrada), y su relación con el abandono.
3. Visualización de relaciones
Heatmap de correlación para identificar relaciones entre todas las variables numéricas.
Scatterplots dirigidos únicamente para los pares con relaciones moderadas o fuertes, evitando una matriz general poco informativa por la cantidad de registros y la naturaleza discreta de varias variables.
4. Evidencia numérica

Se eligió el coeficiente adecuado según el tipo de variables:

Método	Uso en el proyecto
Pearson / Spearman	Numérica ↔ numérica (compras_mes ↔ ingreso_anual, visitas_mes ↔ gasto_publicidad_dirigida)
Punto-biserial	Binaria ↔ numérica (abandono ↔ compras_mes, miembro_premium ↔ ingreso_anual)
V de Cramér	Categórica ↔ categórica (miembro_premium ↔ tipo_dispositivo y region)
5. Interpretación para el negocio

Traducción de los resultados en hallazgos accionables, distinguiendo en cada caso la evidencia visual, la evidencia numérica, lo que sí se puede concluir y lo que no.

💡 Hallazgos principales
1. El ingreso anual depende casi por completo del número de compras

compras_mes e ingreso_anual tienen una correlación casi perfecta (Pearson = Spearman = 0.967). Cada compra adicional eleva el ingreso de forma bastante constante, y los clientes sin compras generan exactamente 0. Ambas variables son prácticamente redundantes, por lo que no deberían incluirse juntas en un mismo modelo. Edad, nivel de ingreso personal y satisfacción no muestran relación con el ingreso generado.

2. La membresía premium se asocia con mucho menor abandono

La tasa de abandono es de 16.81 % en no premium frente a 4.36 % en premium: los no premium abandonan casi 4 veces más. Es la única variable que distingue claramente a quienes se quedan de quienes se van.

3. Los clientes premium generan cerca de 26 % más ingreso sin comprar con mayor frecuencia

Ingreso promedio de 44.58 vs. 35.30 (mediana 37.70 vs. 29.95; r punto-biserial = 0.093, p < 0.0001), con una frecuencia de compra prácticamente igual (1.22 vs. 1.20 al mes). Esto sugiere un ticket promedio más alto entre los premium.

4. El abandono no se explica por actividad, satisfacción, canal ni región

Quienes abandonan compran igual que quienes se quedan (1.23 vs. 1.20; r = 0.008, p = 0.31), la satisfacción casi no se relaciona con el abandono (r = −0.02) y las tasas por dispositivo y región se mantienen entre 14.4 % y 15.8 %. Además, el abandono afecta por igual a clientes de alto y bajo valor.

5. La publicidad dirigida atrae visitas, pero se traduce débilmente en compras

Publicidad ↔ visitas muestra una correlación moderada (Pearson = 0.579; Spearman = 0.559), mientras que publicidad ↔ compras es de solo 0.21. La publicidad explica cerca de un tercio de la variación en las visitas, pero buena parte de ese tráfico no termina en compra.

6. La membresía premium no se concentra en ningún canal ni región

V de Cramér = 0.020 para dispositivo y 0.013 para región, sin asociación relevante. Esto confirma que el efecto premium no está mezclado con diferencias geográficas o de canal.

✅ Conclusiones
El ingreso que genera un cliente se explica, sobre todo, por cuántas veces compra. Aumentar la frecuencia de compra es el camino más directo para elevar el ingreso.
La membresía premium es la palanca más prometedora identificada: sus miembros son más leales y más valiosos. Vale la pena probar campañas de conversión dirigidas a clientes no premium y medir su efecto en el abandono.
El cuello de botella está en la conversión, no en la atracción de tráfico. Antes de aumentar el presupuesto publicitario, conviene revisar la experiencia de compra y medir las campañas por ventas, no solo por visitas.
Con los datos actuales no hay señales de alerta temprana del abandono. Para anticiparlo se necesita información histórica por cliente.

⚠️ Todas las relaciones encontradas son asociaciones, no causalidad.

⚖️ Limitaciones
Correlación ≠ causalidad. Por ejemplo, es posible que los clientes ya leales sean quienes eligen hacerse premium, y no que la membresía cause la retención.
Datos de un solo periodo. El dataset es una foto en el tiempo y no permite observar tendencias, como una caída progresiva de compras antes del abandono.
Posible sesgo de autoselección en el grupo premium.
Grupos pequeños: las conclusiones sobre tablet (1,462 clientes) y sobre clientes con 7 u 8 compras deben interpretarse con cautela.
🚀 Próximos pasos
Segmentación adicional: premium × satisfacción, rangos de edad y nivel de ingreso, clientes sin compras como posible segmento en riesgo, y cuartiles de ingreso_anual para priorizar la retención de clientes de alto valor.
Validar las hipótesis sobre premium: calcular el ingreso por compra para confirmar el ticket promedio más alto y diseñar una prueba piloto de conversión con grupo de control.
Preparar un modelo predictivo de abandono: incorporar datos históricos por cliente (evolución mensual de compras y visitas, antigüedad, contactos con soporte, motivos de baja) y depurar outliers documentando los criterios aplicados.
