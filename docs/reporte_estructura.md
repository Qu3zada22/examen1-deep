# Estructura del reporte tecnico - Grupo 7

**Politica Publica y Asignacion de Recursos - Ciudad UVG**

Extension maxima: 8 paginas. La estructura y los presupuestos de pagina
siguientes son los exigidos por el enunciado del examen; no deben
reordenarse ni omitirse secciones.

---

## 1. Justificacion del paradigma (1 pagina)

- Declarar explicitamente el paradigma elegido: **Dinamica de Sistemas
  (stock-and-flow agregado) combinada con una capa de decision/control
  programada** (la politica de asignacion se reevalua cada bloque de 6
  horas).
- Justificar por que **no** es un Modelo Basado en Agentes: el objeto de
  estudio es el conjunto agregado de recursos, necesidad insatisfecha y
  backlog **por zona**, no un tomador de decisiones individual.
- Justificar por que **no** es Simulacion de Eventos Discretos: no existe
  una disciplina de colas de entidades discretas; la asignacion es una
  decision programada (scheduled), no disparada por eventos.
- Citar explicitamente los criterios de seleccion de paradigma vistos en
  clase (representacion del estado, representacion del tiempo,
  granularidad de las entidades) y enlazar cada criterio con la decision
  tomada.
- Si se combina mas de un paradigma, justificar formalmente la frontera
  entre ellos (en este caso: la frontera entre el nucleo SD y la capa de
  control programada).

**Prompts de apoyo:**
- Que criterio de la diapositiva X descarta ABM para este componente?
- Que criterio descarta DES?
- Donde exactamente esta la frontera entre el stock-and-flow y la capa de
  decision, y por que esa frontera no cruza hacia ABM?

---

## 2. Descripcion del modelo en formato ODD simplificado (2 paginas)

- **Entidades y atributos:** las 5 zonas (Z1..Z5), cada una con su indice
  de dano (0-10), poblacion en riesgo, prioridad declarada
  (CRITICA/ALTA/MODERADA/BAJA) y justificacion; los 8 recursos del archivo
  (presupuesto municipal, presupuesto nacional, vehiculos pesados,
  vehiculos de distribucion, generadores, combustible, kits de agua,
  tiendas de campana), cada uno con su stock inicial (T0), costo unitario y
  fuente de financiamiento.
- **Reglas de comportamiento / ecuaciones de estado:** la actualizacion del
  stock de backlog (S(t+1) = S(t) + inflow*dt - outflow*dt), la
  depreciacion del stock de recursos disponibles, y las 6 politicas de
  asignacion implementadas (equal_share, proportional_to_need,
  severity_weighted, worst_first, threshold_then_proportional,
  manual_plan).
- **Modo de scheduling:** decision de asignacion re-evaluada una vez por
  bloque de 6 horas (12 bloques en total); no hay eventos discretos ni
  colas.
- **Mecanismo de comunicacion entre entidades:** no hay comunicacion
  directa entre zonas; la unica interaccion es a traves del stock de
  recursos compartido (competencia por un pool comun) y, despues del
  intercambio, a traves de los ajustes de necesidad que introducen los
  reportes de los Grupos 2, 3 y 5.
- **Distribucion inicial de atributos:** valores iniciales tomados
  directamente de docs/Grupo7_PoliticaPublica.xlsx (indice de dano,
  poblacion en riesgo, stock T0 por recurso); las tasas de necesidad
  per-capita, la tasa de perdida de acceso y los parametros de
  incertidumbre son supuestos documentados (marcados ASSUMPTION en el
  notebook) porque el archivo no los especifica.

**Recordatorios (campos ODD simplificado a cubrir explicitamente):**
- [ ] Entidades y atributos
- [ ] Reglas de comportamiento / ecuaciones de estado
- [ ] Modo de scheduling
- [ ] Mecanismo de comunicacion
- [ ] Distribucion inicial de atributos

---

## 3. Resultados y analisis (3 paginas)

- Presentar las trayectorias del modelo (cobertura, backlog) con intervalos
  de confianza del 95%, usando los graficos de abanico (fan chart)
  generados en la Seccion 12 del notebook.
- Identificar los momentos criticos del sistema: cuando se satura, cuando
  colapsa, cuando se estabiliza (usar las trayectorias de coverage y
  backlog por zona).
- Incluir el analisis del output requerido en la seccion 6 del archivo
  Excel: el plan de asignacion de 72 horas (item a), la justificacion
  cuantitativa via el registro de auditoria (item b), y la evaluacion
  contra las 4 metricas con umbrales y pass/fail (item c).
- Responder explicitamente las dos preguntas del grupo:
  - **Pregunta 1:** la propuesta inicial de asignacion para las primeras
    24 horas (Seccion 13 del notebook) y cual metrica es la mas dificil de
    cumplir y por que.
  - **Pregunta 2:** el efecto de incorporar los reportes de los Grupos 2, 3
    y 5 (Seccion 14), la decision que mas cambio, y el impacto cuantificado
    sobre al menos dos de las cuatro metricas.

**Recordatorios de los pesos de evaluacion de las 4 metricas** (deben
citarse explicitamente al interpretar los resultados):
- M1 tasa de mortalidad evitable - peso 30% - umbral < 15%
- M2 tiempo promedio de respuesta (zonas CRITICA) - peso 25% - umbral < 6h
- M3 cobertura de suministros en 24h - peso 25% - umbral > 80%
- M4 eficiencia presupuestaria - peso 20% - umbral > 850 personas/MQ

---

## 4. Incorporacion del intercambio presencial (1 pagina)

- Documentar que recibio el grupo en el intercambio presencial (reportes de
  Grupos 2, 3 y 5) y en que formato exacto llegaron (o si se uso el
  placeholder sintetico del notebook porque los archivos reales aun no
  estaban disponibles).
- Explicar como se incorporo cada reporte al modelo (funcion
  Exchange.need_adjustment, Seccion 6 del notebook) y documentar cualquier
  contradiccion entre la informacion recibida y los supuestos propios, y
  como se resolvio esa contradiccion.
- Reportar que cambio en las conclusiones despues de incorporar el
  intercambio (tabla de la Seccion 14 del notebook, con deltas e
  intervalos de confianza del 95%).

---

## 5. Limitaciones y propuestas de mejora (1 pagina)

Identificar al menos dos limitaciones estructurales del modelo. Puntos ya
identificados durante el desarrollo (usar como punto de partida, revisar
antes de copiar):

- Las tasas de necesidad per-capita (Seccion 4 del notebook,
  baseline_need_per_capita) son supuestos calibrados, no datos del archivo
  Excel; con mas tiempo se solicitarian al comite de emergencia tasas
  reales por tipo de recurso.
- La restriccion C4 (coordinacion para vehiculos pesados) se modela con una
  probabilidad de disponibilidad de ruta inventada
  (route_availability_probability) porque el Grupo 7 no recibe
  directamente el mapa de accesibilidad del Grupo 4; con mas datos, esta
  probabilidad deberia derivarse del indice de aislamiento real de esa
  zona.
- La metrica M4 (eficiencia presupuestaria) puede mostrar comportamiento
  contraintuitivo bajo la calibracion actual (ver la nota de la Seccion 14b
  del notebook: reducir el presupuesto nacional a la mitad puede aumentar
  la eficiencia calculada) porque el presupuesto nacional domina el gasto
  total mucho mas de lo que impulsa la cobertura agregada; esto es una
  limitacion de calibracion, no del diseno del modelo, y debe corregirse
  cuando se reemplacen los supuestos por datos reales.
- Proponer como se resolverian estas limitaciones con mas tiempo o datos
  (ej. datos reales de necesidad per-capita, el mapa de accesibilidad del
  Grupo 4 en lugar de una probabilidad supuesta).
