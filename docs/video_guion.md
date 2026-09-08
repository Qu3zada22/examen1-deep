# Guion del video de presentacion - Grupo 7

**Politica Publica y Asignacion de Recursos - Ciudad UVG**

Duracion total: 3 a 5 minutos. Debe mostrar el modelo en funcionamiento
(pantalla del notebook o de las graficas generadas) y al menos un
integrante del grupo hablando en camara. No se aceptan videos de solo
pantalla sin voz ni guiones leidos.

Los cuatro puntos deben presentarse en el orden indicado, con los tiempos
exactos exigidos por el enunciado.

---

## Punto 1: Decision de diseno mas dificil (45 segundos)

Explicar cual fue la decision de diseno mas dificil que tomo el grupo
(paradigma, scheduling, inicializacion, regla de comportamiento) y por que
se tomo asi. No describir el modelo completo, concentrarse solo en esa
decision.

**Bullets de apoyo para preparar la respuesta:**
- Candidata sugerida: como agregar el backlog de 8 recursos con unidades
  incompatibles (quetzales en millones vs. unidades fisicas) sin que el
  presupuesto domine artificialmente el indice de urgencia -> decision de
  usar un indice de backlog adimensional (proporcion relativa de faltante)
  en vez de sumar unidades crudas.
- Alternativa: la decision de calibrar las tasas de necesidad per-capita
  (no dadas por el Excel) para que la escasez fuera severa pero no
  absoluta, de modo que las politicas de asignacion pudieran diferenciarse
  entre si.
- Justificar la decision elegida con el razonamiento propio del grupo, no
  solo repetir lo que genero una IA.

---

## Punto 2: Resultado mas importante (60 segundos)

Mostrar en pantalla el grafico o tabla mas importante de los resultados
(sugerido: la tabla de evaluacion contra las 4 metricas con intervalos de
confianza del 95%, o el grafico de trayectorias de cobertura por zona) y
explicarlo verbalmente. Debe mencionarse el intervalo de confianza y que
implica para la incertidumbre de la conclusion.

**Bullets de apoyo:**
- Que metrica(s) cumplen el umbral y cuales no.
- Cual es el ancho del intervalo de confianza del 95% y que tan seguros
  estan de la conclusion presentada.
- Por que se eligio ese grafico/tabla como "el resultado mas importante".

---

## Punto 3: El intercambio presencial (60 segundos)

Explicar que informacion se recibio de otro grupo (Grupo 2, 3 o 5, o los
tres), como se incorporo al modelo, y si eso cambio o confirmo las
conclusiones previas al intercambio.

**Bullets de apoyo:**
- Que reporte(s) llegaron realmente y en que formato.
- Que decision de asignacion cambio mas despues de incorporar esa
  informacion.
- Cuanto cambio (usar la tabla de deltas con intervalos de confianza de la
  Seccion 14 del notebook).

---

## Punto 4: Pregunta abierta (45 segundos)

Pregunta asignada al Grupo 7 (transcrita textualmente del enunciado del
examen):

> "Su modelo produce una recomendacion de asignacion de recursos que
> maximiza alguna combinacion de las metricas definidas, pero toda metrica
> es una simplificacion de lo que realmente importa en una emergencia.
> Que dimension del bienestar humano durante el desastre no esta
> capturada en ninguna de las cuatro metricas de su archivo, y si esa
> dimension se incorporara formalmente, que decision de asignacion
> cambiaria y a favor de que zona?"

**Bullets de apoyo para preparar la respuesta:**
- Elegir una dimension concreta no capturada por M1-M4 (ej. bienestar
  psicologico/trauma, cohesion social y desplazamiento familiar, dignidad
  en el refugio temporal, equidad intergeneracional).
- Explicar por que ninguna de las 4 metricas (mortalidad evitable, tiempo
  de respuesta, cobertura de suministros, eficiencia presupuestaria) captura
  esa dimension.
- Argumentar concretamente que decision de asignacion cambiaria si esa
  dimension se incorporara formalmente, y a favor de que zona (usar el
  indice de dano y la prioridad declarada de las zonas CRITICA, Z1 y Z5,
  como punto de partida del argumento).
- Evitar una respuesta generica; debe conectarse con los resultados
  concretos del modelo del grupo.
