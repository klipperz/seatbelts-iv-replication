# seatbelts-iv-replication
Replicación de Cohen &amp; Einav (2003) — efecto causal del cinturón de seguridad sobre fatalidades viales con variables instrumentales en R
# Replicación de Cohen & Einav (2003) — Variables instrumentales y fatalidades viales

Proyecto de econometría aplicada desarrollado en R para replicar la estrategia empírica de Cohen & Einav (2003),
evaluando el efecto causal del uso del cinturón de seguridad sobre las fatalidades en accidentes de tránsito
en Estados Unidos mediante un modelo de variables instrumentales.

---

# Ver el análisis completo

El documento completo con código, resultados, gráficos e interpretaciones está disponible acá:

[Ver análisis](https://htmlpreview.github.io/?https://github.com/klipperz/seatbelts-iv-replication/blob/main/quizz-3.html)

El código fuente en R Markdown está en `quizz 3.Rmd`.

---

# Pregunta central

¿Cuánto reduce realmente el uso del cinturón de seguridad las muertes en accidentes de tránsito?
El problema es que esta relación no puede estimarse directamente: los conductores no usan el cinturón de
forma aleatoria, lo que introduce endogeneidad y sesga cualquier estimación por MCO.

---

# Objetivo del estudio

Aislar el efecto causal del uso del cinturón sobre las fatalidades de ocupantes, corrigiendo la endogeneidad
mediante variables instrumentales.

El proyecto busca responder:

- ¿Por qué MCO no sirve para estimar este efecto?
- ¿Qué tan fuerte es el efecto protector del cinturón una vez corregido el sesgo?
- ¿Son válidos los instrumentos utilizados?

---

# Metodología

El análisis utiliza un panel de datos de los 51 estados de EE.UU. para el período 1983–1997.

## Características del modelo

- Variable dependiente: logaritmo de la tasa de fatalidades de ocupantes
- Variable endógena: logaritmo del uso del cinturón de seguridad
- Instrumentos: dummies de leyes de aplicación primaria y secundaria del cinturón
- Efectos fijos por estado
- Identificación estructural mediante mínimos cuadrados en dos etapas (MC2E)

## Variables incluidas

- Tasa de uso del cinturón (sb_useage)
- Tasa de fatalidades de ocupantes (fatalityrate)
- Límites de velocidad (speed65, speed70)
- Edad mínima para beber alcohol (drinkage21)
- Límite de alcohol en sangre (ba08)
- Ingreso per cápita (income)
- Millas recorridas en zonas rurales y urbanas (vmtrural, vmturban)

---

# Análisis descriptivo

Se analiza la correlación entre uso del cinturón y fatalidades en tres años de referencia (1983, 1990, 1997),
mostrando visualmente por qué una regresión simple produce resultados engañosos.

---

# Diagnóstico de los instrumentos

## Test de instrumentos débiles

Estadístico F = 71.61 (p < 0.001). Los instrumentos están fuertemente correlacionados con el uso del cinturón.

## Test de Sargan

p = 0.348. No se rechaza la exogeneidad de los instrumentos.

## Test de Wu-Hausman

Contraste entre MCO y MC2E para evaluar la relevancia práctica de la corrección por endogeneidad.

---

# Resultados principales

La corrección por endogeneidad prácticamente duplica el efecto estimado:

- MCO: –0.036 (marginalmente significativo)
- MC2E: –0.079 (significativo al 10%)

Esto es consistente con sesgo de atenuación por causalidad inversa en el estimador MCO:
los conductores tienden a usar más el cinturón en condiciones de mayor riesgo percibido,
lo que comprime artificialmente el coeficiente hacia cero.

---

# Discusión crítica

Se discute la tensión en la condición de exogeneidad de los instrumentos: las leyes de cinturón
no se aprueban de forma aleatoria, sino como resultado de procesos políticos endógenos que pueden
estar correlacionados con otras variables que afectan las fatalidades directamente.

---

# Software utilizado

- R (tidyverse, AER, modelsummary, gt)

# Fuente de los datos

- Cohen, A. & Einav, L. (2003). Dataset de panel estatal EE.UU. 1983–1997. Disponible en el paquete `AER` de R.

# Referencias

- Cohen, A., & Einav, L. (2003). The effects of mandatory seat belt laws on driving behavior and traffic fatalities. *Review of Economics and Statistics*, 85(4), 828–843.
- Angrist, J. & Pischke, J.S. (2009). *Mostly Harmless Econometrics*. Princeton University Press.
