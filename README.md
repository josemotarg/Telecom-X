# Telecom-X
Evasión de cliente

## Objetivo del análisis y el problema de evasión de clientes (Churn).

**Introducción:**

El objetivo de este análisis es explorar y comprender los factores que influyen en la evasión de clientes, comúnmente conocida como "Churn", en una empresa de telecomunicaciones. El Churn representa la pérdida de clientes que deciden interrumpir su servicio con la compañía. Identificar y predecir el Churn es crucial para la salud financiera y el crecimiento de la empresa, ya que adquirir nuevos clientes suele ser más costoso que retener a los existentes.

En este cuaderno, analizaremos un conjunto de datos que contiene información sobre los clientes de una compañía de telecomunicaciones, incluyendo sus datos demográficos, servicios contratados, detalles de la cuenta y si han dejado la compañía o no. Utilizaremos técnicas de visualización y análisis exploratorio de datos para identificar patrones, tendencias y características asociadas con el comportamiento de Churn. Esto nos permitirá obtener insights valiosos para desarrollar estrategias efectivas de retención de clientes.

## Pasos realizados para importar, limpiar y procesar los datos.

**Importación de librerías:** Se importaron las librerías necesarias: requests para obtener datos de la web, pandas para la manipulación y análisis de datos, y plotly.express para visualizaciones interactivas.

**Descarga de datos:** Se utilizó la librería requests para descargar el archivo JSON desde una URL específica.

**Carga a DataFrame:** El contenido JSON descargado se convirtió a un diccionario Python y luego se cargó en un DataFrame de pandas (df). Adicionalmente, se utilizó pd.json_normalize para aplanar la estructura JSON anidada y crear un DataFrame más plano (df_plano), que es el que se utilizó para el análisis.

**Inspección inicial:**
df_plano.info(): Se revisó la información general del DataFrame, incluyendo los tipos de datos de cada columna y la presencia de valores no nulos.
Se verificó la cantidad de valores únicos en cada columna y se imprimieron los valores únicos para columnas con menos de 50 valores distintos para comprender la naturaleza de los datos categóricos y numéricos.

**Manejo de duplicados:** Se verificó la existencia de filas duplicadas en el DataFrame.

**Manejo de valores nulos y vacíos:**
Se identificaron los valores nulos (NaN) en cada columna utilizando df_plano.isnull().sum().
Se identificaron las celdas con strings vacíos o en blanco después de eliminar espacios (df_plano.apply(lambda x: x.astype(str).str.strip() == '').sum()).

**Conversión de tipo de dato:** La columna 'account.Charges.Total', que inicialmente podría ser de tipo objeto debido a la presencia de strings vacíos, se convirtió al tipo numérico flotante (float). Se utilizó errors='coerce' para convertir los valores que no pudieron ser parseados (como los strings vacíos) a NaN.

**Eliminación de filas con valores vacíos en 'Churn':** Se filtraron las filas donde la columna 'Churn' contenía strings vacíos después de eliminar espacios, ya que estos registros no aportaban información sobre el estado de Churn.

**Descripción estadística:** Se generó un resumen estadístico de las columnas numéricas (df_plano.describe()) para obtener información sobre la distribución de los datos (media, desviación estándar, cuartiles, etc.).

**Creación de nueva columna (temporal):** Se calculó una columna 'Cuentas_Diarias' dividiendo 'account.Charges.Monthly' por 30. Aunque esta columna se calculó, posteriormente se eliminó (df_plano = df_plano.drop('Cuentas_Diarias', errors='ignore')), sugiriendo que fue un cálculo exploratorio inicial o no se consideró relevante para el análisis final.

**Feature Engineering (Número de Servicios Adicionales):** Se creó una nueva columna llamada num_servicios_adicionales sumando el número de servicios adicionales (MultipleLines, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies) contratados por cada cliente. Esto se hizo para explorar la relación entre la cantidad de servicios y la probabilidad de Churn.

## Análisis Exploratorio de Datos (EDA):
A continuación, se presenta una explicación de los análisis exploratorios de datos realizados sobre el dataset de clientes de telecomunicaciones:

**Análisis Descriptivo Inicial:**
Se cargaron los datos y se normalizó la estructura JSON anidada para obtener un DataFrame plano (df_plano).
Se realizó una inspección inicial utilizando df_plano.info() para comprender los tipos de datos y la presencia de valores no nulos.
Se verificaron los valores únicos en cada columna para identificar variables categóricas y sus posibles valores, así como para entender la granularidad de las variables numéricas.
Se comprobó la existencia de filas duplicadas y se verificó la presencia de valores nulos y vacíos (strings en blanco) en el dataset.
Se identificó que la columna 'account.Charges.Total' contenía strings vacíos, lo que impedía su uso como número. Se realizó la conversión a tipo flotante, manejando los errores (errors='coerce') para convertir los valores no parseables a NaN.
Se eliminaron las filas donde la columna 'Churn' contenía strings vacíos, ya que estos registros no proporcionaban información sobre el estado de Churn.
Se obtuvo un resumen estadístico de las columnas numéricas (df_plano.describe()) para entender la distribución, media, desviación estándar, etc.

**Análisis de Churn por Género:**
Se utilizó un histograma (px.histogram) para visualizar la distribución de clientes por género (customer.gender) y, dentro de cada género, la cantidad de clientes que han hecho Churn. Esto permite observar si existe una diferencia notable en la tasa de Churn entre hombres y mujeres.

**Análisis de Churn por Situación de Ciudadano Senior:**
Se visualizó la distribución de clientes por su estado de ciudadano senior (customer.SeniorCitizen) en relación con el Churn. Esto ayuda a determinar si ser un ciudadano senior influye en la probabilidad de dejar la compañía.

**Análisis de Churn por Antigüedad (Tenure):**
Se analizó la relación entre la antigüedad del cliente (customer.tenure, que representa el número de meses que un cliente ha estado con la compañía) y el Churn.
Se calculó la tasa de Churn (%) para cada valor de antigüedad agrupando por customer.tenure y Churn.
Se visualizó la tasa de Churn a lo largo del tiempo utilizando un gráfico de líneas (px.line). Este gráfico es crucial para identificar si los clientes con mayor o menor antigüedad tienen una mayor o menor propensión al Churn.

**Análisis de la Relación entre Antigüedad, Cargos Mensuales y Churn:**
Se utilizó un gráfico de dispersión (px.scatter) para visualizar la relación entre la antigüedad del cliente (customer.tenure), los cargos mensuales (account.Charges.Monthly) y el estado de Churn. Esto permite observar si hay patrones en los cargos mensuales para diferentes antigüedades y si estos patrones se relacionan con el Churn.

**Análisis de Churn por Tipo de Contrato:**
Se visualizó la distribución de clientes por tipo de contrato (account.Contract) y su relación con el Churn utilizando un histograma. Se espera que diferentes tipos de contrato (mensual, anual, bianual) tengan distintas tasas de Churn.

**Análisis de la Distribución de Cargos Mensuales por Churn:**
Se utilizó un gráfico de caja (px.box) para visualizar la distribución de los cargos mensuales (account.Charges.Monthly) para los clientes que hicieron Churn y los que no. Esto ayuda a identificar si los clientes que se van tienden a tener cargos mensuales más altos o más bajos.

**Análisis de Churn por Servicios de Teléfono e Internet:**
Se examinó la relación entre tener servicio telefónico (phone.PhoneService) y servicio de internet (internet.InternetService) con el Churn utilizando histogramas. Esto permite ver si la presencia o el tipo de estos servicios básicos influyen en la decisión del cliente de irse.

**Análisis de Churn por Servicios de Streaming (TV y Películas):**
Se visualizó la relación entre tener servicios de streaming de TV (internet.StreamingTV) y películas (internet.StreamingMovies) con el Churn. Esto ayuda a entender si estos servicios adicionales de entretenimiento están asociados con la retención o la pérdida de clientes.

**Análisis de Churn por Método de Pago:**
Se analizó la distribución de clientes por su método de pago (account.PaymentMethod) y su relación con el Churn utilizando un histograma. Diferentes métodos de pago podrían estar asociados con distintas tasas de Churn.

**Análisis del Número de Servicios Adicionales y Churn:**
Se realizó un análisis para entender cómo la cantidad de servicios adicionales contratados por un cliente afecta la probabilidad de Churn.
Se creó una nueva característica (num_servicios_adicionales) sumando el número de servicios adicionales que cada cliente tiene.
Se calculó la tasa de Churn (%) para cada número de servicios adicionales.
Se visualizó la tasa de Churn por el número de servicios adicionales utilizando un gráfico de líneas (px.line) y la distribución del número de servicios adicionales por estado de Churn utilizando un histograma (px.histogram). Este análisis ayuda a determinar si los clientes con más servicios adicionales son más propensos a quedarse o a irse.
En resumen, el EDA realizado ha permitido identificar varias características clave asociadas con el Churn, como la antigüedad del cliente, el tipo de contrato, los cargos mensuales, y la cantidad y tipo de servicios contratados. Estos hallazgos son fundamentales para el desarrollo de modelos predictivos de Churn y para la implementación de estrategias de retención dirigidas a segmentos de clientes de alto riesgo.

## Conclusiones e Insights:
Basado en el análisis exploratorio de datos (EDA) realizado, los principales hallazgos que pueden ayudar a reducir la evasión de clientes (Churn) son los siguientes:

**Antigüedad del Cliente (Tenure):**
Insight: Los clientes con menor antigüedad (especialmente en los primeros meses) presentan una tasa de Churn significativamente más alta. La tasa de Churn tiende a disminuir a medida que aumenta la antigüedad del cliente.
Aplicación para Reducir la Evasión: Se deben enfocar esfuerzos de retención intensivos en los clientes nuevos, ofreciendo programas de bienvenida, seguimiento proactivo, soporte al cliente excepcional y asegurándose de que tengan una experiencia inicial positiva. Considerar ofertas especiales o descuentos para clientes en sus primeros meses para incentivarlos a quedarse.

**Tipo de Contrato:**
Insight: Los clientes con contratos mensuales tienen una tasa de Churn mucho mayor en comparación con aquellos con contratos anuales o bianuales. Los contratos a largo plazo actúan como un fuerte factor de retención.
Aplicación para Reducir la Evasión: Incentivar a los clientes a cambiar de contratos mensuales a contratos a más largo plazo. Esto se puede lograr ofreciendo descuentos significativos, paquetes de servicios adicionales gratuitos, o mejores tarifas para aquellos que se comprometan por un período más largo.

**Cargos Mensuales:**
Insight: Los clientes que hacen Churn tienden a tener cargos mensuales promedio más altos que los clientes que se quedan. Esto podría indicar que perciben que el costo no se alinea con el valor que reciben, o que encuentran ofertas más económicas en la competencia.
Aplicación para Reducir la Evasión: Monitorear de cerca a los clientes con cargos mensuales altos. Ofrecerles revisiones de su plan para asegurarse de que están en el más adecuado para sus necesidades, o proponerles paquetes que les proporcionen un mejor valor percibido por el precio. También, realizar análisis de precios competitivos para asegurar que las tarifas son atractivas.

**Servicios de Internet:**
Insight: Los clientes con servicio de internet, especialmente aquellos con planes como Fiber Optic, pueden tener tasas de Churn diferentes a los que no tienen servicio de internet o tienen DSL. La presencia y el tipo de servicio de internet son factores relevantes.
Aplicación para Reducir la Evasión: Analizar específicamente los motivos de Churn para los clientes con diferentes tipos de servicio de internet. Mejorar la calidad y la estabilidad del servicio de internet, especialmente para aquellos con planes de alta velocidad, ya que problemas en este servicio pueden ser una causa importante de frustración y evasión.

**Método de Pago:**
Insight: Ciertos métodos de pago podrían estar asociados con mayores tasas de Churn (por ejemplo, pagos electrónicos vs. cheques por correo).
Aplicación para Reducir la Evasión: Investigar por qué los clientes que usan métodos de pago con alta tasa de Churn son más propensos a irse. Podría estar relacionado con la comodidad del pago, problemas con las transacciones, o simplemente ser un indicador de un segmento de clientes menos comprometidos. Promover métodos de pago más convenientes y confiables (como la domiciliación bancaria automática) que pueden reducir la fricción en la relación cliente-empresa.

**Número de Servicios Adicionales:**
Insight: Los clientes que tienen un mayor número de servicios adicionales contratados tienden a tener una menor tasa de Churn. Esto sugiere que cuantos más servicios un cliente tiene, más "conectado" está con la empresa y más valor percibe.
Aplicación para Reducir la Evasión: Fomentar la venta cruzada y la agrupación de servicios. Ofrecer paquetes que incluyan múltiples servicios a un precio atractivo puede aumentar la "stickiness" del cliente y reducir la probabilidad de Churn. Identificar a los clientes con pocos servicios adicionales como potenciales candidatos para ofertas de upselling.

**Cómo estos datos pueden ayudar a reducir la evasión:**
Identificación Temprana de Clientes en Riesgo: Los insights sobre la antigüedad, el tipo de contrato y los cargos mensuales permiten identificar proactivamente a los clientes que exhiben características asociadas con un alto riesgo de Churn.
Personalización de Estrategias de Retención: En lugar de aplicar una estrategia de retención genérica, la empresa puede adaptar sus esfuerzos basándose en los factores de riesgo identificados. Por ejemplo, ofrecer programas de fidelización a largo plazo a clientes con contratos mensuales, o revisar la facturación con clientes de altos cargos mensuales.
Mejora de la Experiencia del Cliente: Al entender qué servicios o características (como el tipo de internet o la cantidad de servicios adicionales) están relacionados con el Churn, la empresa puede invertir en mejorar la calidad de esos servicios o en promover la adopción de servicios que aumentan la retención.
Desarrollo de Ofertas Dirigidas: Se pueden diseñar ofertas y promociones específicas para los segmentos de clientes de alto riesgo, como descuentos por cambio a contratos a largo plazo, o paquetes de servicios adicionales a precios reducidos.
Optimización de Canales de Comunicación: Entender qué métodos de pago o interacciones están asociados con el Churn puede ayudar a optimizar los canales de comunicación y soporte para abordar problemas potenciales antes de que lleven a la evasión.
En resumen, este análisis proporciona una base sólida para comprender el comportamiento de Churn. La empresa puede utilizar estos hallazgos para construir modelos predictivos más sofisticados, implementar campañas de retención dirigidas y mejorar continuamente la experiencia del cliente basándose en datos. La clave está en pasar de la identificación a la acción, utilizando estos insights para intervenir proactivamente y aumentar la lealtad del cliente.

## Recomendaciones Estratégicas para Reducir el Churn:
Basándonos en los insights obtenidos del análisis exploratorio de datos, aquí se presentan algunas recomendaciones estratégicas para mitigar la tasa de Churn:

**Programa de Bienvenida y Onboarding Reforzado:**
Objetivo: Reducir el alto Churn en los primeros meses.
Acción: Implementar un programa de onboarding estructurado y proactivo para los nuevos clientes. Incluir comunicación regular en las primeras semanas (emails, SMS, llamadas) para asegurar que el cliente se sienta cómodo con los servicios, resolver dudas tempranas y ofrecer soporte. Considerar la asignación de un "embajador de cliente" para los primeros meses de los nuevos suscriptores.

**Incentivos para Contratos a Largo Plazo:**
Objetivo: Convertir clientes de contratos mensuales a contratos anuales o bianuales para aumentar la retención.
Acción: Ofrecer descuentos significativos en la tarifa mensual, instalación gratuita de servicios adicionales, o un mes de servicio gratis al cambiar de un contrato mensual a uno a más largo plazo. Comunicar claramente los beneficios y el ahorro a largo plazo de los contratos extendidos.

**Monitoreo y Ofertas Dirigidas para Clientes con Cargos Altos:**
Objetivo: Abordar la posible percepción de falta de valor en clientes con altos cargos mensuales.
Acción: Implementar un sistema de alerta para identificar clientes con cargos mensuales por encima de un cierto umbral. Contactar proactivamente a estos clientes para revisar su uso, sugerir optimizaciones de plan, o ofrecer paquetes de servicios adicionales que incrementen el valor percibido por el mismo o un costo ligeramente mayor. Realizar un análisis de precios de la competencia para asegurar la competitividad de los planes de alta gama.

**Mejora Continua de la Calidad del Servicio de Internet, Especialmente Fibra Óptica:**
Objetivo: Reducir la frustración y el Churn asociado con problemas en el servicio de internet de alta velocidad.
Acción: Invertir en infraestructura para garantizar la estabilidad y velocidad de la conexión de fibra óptica. Establecer canales de soporte técnico prioritarios para usuarios de fibra. Monitorear proactivamente la calidad de la conexión y notificar a los clientes sobre cualquier interrupción o mantenimiento planificado.

**Optimización de Métodos de Pago y Comunicación:**
Objetivo: Reducir el Churn potencialmente asociado con ciertos métodos de pago.
Acción: Analizar los métodos de pago con mayor tasa de Churn para entender las causas raíz. Si es por conveniencia, promover activamente la domiciliación bancaria automática (AutoPay) ofreciendo un pequeño descuento o beneficio. Si es por problemas técnicos, mejorar la plataforma de pago online. Asegurar que la comunicación relacionada con la facturación sea clara y sin errores.

**Estrategia de Venta Cruzada y Agrupación de Servicios (Bundling):**
Objetivo: Aumentar el número de servicios por cliente para incrementar la "stickiness".
Acción: Diseñar paquetes de servicios (internet + TV + teléfono, internet + seguridad online, etc.) que ofrezcan un mejor valor que la contratación de servicios individuales. Capacitar al personal de ventas y soporte para identificar oportunidades de venta cruzada. Ofrecer promociones especiales para clientes con pocos servicios adicionales para incentivarlos a agregar más.

**Implementación de un Sistema de Alerta Temprana de Churn:**
Objetivo: Identificar a los clientes en riesgo antes de que decidan irse.
Acción: Desarrollar un modelo predictivo de Churn utilizando los insights identificados (tenure bajo, contrato mensual, altos cargos, etc.) y posiblemente otras variables. Implementar un sistema que genere alertas cuando un cliente muestre un comportamiento de riesgo (ej. disminución de uso de servicios, múltiples llamadas al soporte técnico por el mismo problema, interacción negativa con la empresa).

**Programas de Fidelización y Recompensas:**
Objetivo: Reconocer y recompensar a los clientes leales, especialmente aquellos con mayor antigüedad y múltiples servicios.
Acción: Crear un programa de puntos, descuentos exclusivos, acceso anticipado a nuevas tecnologías, o servicio al cliente preferencial para clientes de alto valor y larga permanencia. Esto ayuda a que los clientes se sientan valorados y menos propensos a buscar alternativas.

**Recopilación Activa de Feedback del Cliente:**
Objetivo: Entender las razones detrás de la insatisfacción antes de que lleven al Churn.
Acción: Implementar encuestas de satisfacción del cliente en puntos clave de la interacción (ej. después de una llamada de soporte, después de la instalación de un nuevo servicio). Monitorear activamente las redes sociales y plataformas de reseñas. Utilizar este feedback para identificar áreas de mejora operativa y de servicio.
La implementación de estas recomendaciones requiere un esfuerzo coordinado entre diferentes departamentos (marketing, ventas, operaciones, servicio al cliente). La clave es utilizar los datos y los insights para pasar de un enfoque reactivo a uno proactivo en la gestión de la relación con el cliente, centrándose en retener a los clientes valiosos antes de que consideren la opción de Churn.
