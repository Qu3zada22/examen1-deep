# Estructura del reporte técnico - Grupo 7

Política Pública y Asignación de Recursos - Ciudad UVG

Extensión máxima: 8 páginas. La estructura y los presupuestos de página
siguientes son los exigidos por el enunciado del examen; no deben
reordenarse ni omitirse secciones.

---

## 1. Justificación del paradigma (1 página)

- Declarar explícitamente el paradigma elegido: Dinámica de Sistemas
  (stock-and-flow agregado) combinada con una capa de decisión/control
  programada (la política de asignación se reevalúa cada bloque de 6
  horas).
- Justificar por qué no es un Modelo Basado en Agentes: el objeto de
  estudio es el conjunto agregado de recursos, necesidad insatisfecha y
  backlog por zona, no un tomador de decisiones individual.
- Justificar por qué no es Simulación de Eventos Discretos: no existe
  una disciplina de colas de entidades discretas; la asignación es una
  decisión programada (scheduled), no disparada por eventos.
- Citar explícitamente los criterios de selección de paradigma vistos en
  clase (representación del estado, representación del tiempo,
  granularidad de las entidades) y enlazar cada criterio con la decisión
  tomada.
- Si se combina más de un paradigma, justificar formalmente la frontera
  entre ellos (en este caso: la frontera entre el núcleo SD y la capa de
  control programada).

Prompts de apoyo:
- Qué criterio de la diapositiva X descarta ABM para este componente?
- Qué criterio descarta DES?
- Dónde exactamente está la frontera entre el stock-and-flow y la capa de
  decisión, y por qué esa frontera no cruza hacia ABM?

---

## 2. Descripción del modelo en formato ODD simplificado (2 páginas)

- Entidades y atributos: las 5 zonas (Z1..Z5), cada una con su índice
  de daño (0-10), población en riesgo, prioridad declarada
  (CRÍTICA/ALTA/MODERADA/BAJA) y justificación; los 8 recursos del archivo
  (presupuesto municipal, presupuesto nacional, vehículos pesados,
  vehículos de distribución, generadores, combustible, kits de agua,
  tiendas de campaña), cada uno con su stock inicial (T0), costo unitario y
  fuente de financiamiento.
- Reglas de comportamiento y ecuaciones de estado: la actualización del
  stock de backlog (S(t+1) = S(t) + inflow*dt - outflow*dt), la
  depreciación del stock de recursos disponibles, y las 6 políticas de
  asignación implementadas (equal_share, proportional_to_need,
  severity_weighted, worst_first, threshold_then_proportional,
  score_prioridad; más manual_plan como plantilla manual auxiliar).
- Modo de scheduling: decisión de asignación re-evaluada una vez por
  bloque de 6 horas (12 bloques en total); no hay eventos discretos ni
  colas.
- Mecanismo de comunicación entre entidades: no hay comunicación
  directa entre zonas; la única interacción es a través del stock de
  recursos compartido (competencia por un pool común) y, después del
  intercambio, a través de los ajustes de necesidad que introducen los
  reportes de los Grupos 2, 3 y 5.
- Distribución inicial de atributos: valores iniciales tomados
  directamente de docs/Grupo7_PoliticaPublica.xlsx (índice de daño,
  población en riesgo, stock T0 por recurso); las tasas de necesidad
  per cápita, la tasa de pérdida de acceso y los parámetros de
  incertidumbre son supuestos documentados (marcados ASSUMPTION en el
  notebook) porque el archivo no los especifica.

Recordatorios (campos ODD simplificado a cubrir explícitamente):
- [ ] Entidades y atributos
- [ ] Reglas de comportamiento / ecuaciones de estado
- [ ] Modo de scheduling
- [ ] Mecanismo de comunicación
- [ ] Distribución inicial de atributos

---

## 3. Resultados y análisis (3 páginas)

- Presentar las trayectorias del modelo (cobertura, backlog) con intervalos
  de confianza del 95%, usando los gráficos de abanico (fan chart)
  generados en la Sección 4.9 del notebook.
- Identificar los momentos críticos del sistema: cuándo se satura, cuándo
  colapsa, cuándo se estabiliza (usar las trayectorias de coverage y
  backlog por zona).
- Incluir el análisis del output requerido en la sección 6 del archivo
  Excel: el plan de asignación de 72 horas (item a), la justificación
  cuantitativa vía el registro de auditoría (item b), y la evaluación
  contra las 4 métricas con umbrales y pass/fail (item c).
- Responder explícitamente las dos preguntas del grupo:
  - Pregunta 1: la propuesta inicial de asignación para las primeras
    24 horas (Sección 5 del notebook) y cuál métrica es la más difícil de
    cumplir y por qué.
  - Pregunta 2: el efecto de incorporar los reportes de los Grupos 2, 3
    y 5 (Sección 7), la decisión que más cambió, y el impacto cuantificado
    sobre al menos dos de las cuatro métricas.

Recordatorios de los pesos de evaluación de las 4 métricas (deben citarse
explícitamente al interpretar los resultados):
- M1 tasa de mortalidad evitable, peso 30%, umbral < 15%
- M2 tiempo promedio de respuesta (zonas CRÍTICA), peso 25%, umbral < 6h
- M3 cobertura de suministros en 24h, peso 25%, umbral > 80%
- M4 eficiencia presupuestaria, peso 20%, umbral > 850 personas/MQ

---

## 4. Incorporación del intercambio presencial (1 página)

- Documentar qué recibió el grupo en el intercambio presencial (reportes de
  Grupos 2, 3 y 5, Sección 6 del notebook) y en qué formato exacto llegaron
  (o si se usó el placeholder sintético del notebook porque los archivos
  reales aún no estaban disponibles).
- Explicar cómo se incorporó cada reporte al modelo (función
  Exchange.need_adjustment, Sección 4.3 del notebook) y documentar cualquier
  contradicción entre la información recibida y los supuestos propios, y
  cómo se resolvió esa contradicción.
- Reportar qué cambió en las conclusiones después de incorporar el
  intercambio (tabla de la Sección 7 del notebook, con deltas e
  intervalos de confianza del 95%).

---

## 5. Limitaciones y propuestas de mejora (1 página)

Identificar al menos dos limitaciones estructurales del modelo. Puntos ya
identificados durante el desarrollo (usar como punto de partida, revisar
antes de copiar; ver también la Sección 10 del notebook):

- Las tasas de necesidad per cápita (Sección 4.1 del notebook,
  baseline_need_per_capita) son supuestos calibrados, no datos del archivo
  Excel; con más tiempo se solicitarían al comité de emergencia tasas
  reales por tipo de recurso.
- La restricción C4 (coordinación para vehículos pesados) se modela con una
  probabilidad de disponibilidad de ruta inventada
  (route_availability_probability) porque el Grupo 7 no recibe
  directamente el mapa de accesibilidad del Grupo 4; con más datos, esta
  probabilidad debería derivarse del índice de aislamiento real de esa
  zona.
- La métrica M4 (eficiencia presupuestaria) puede mostrar comportamiento
  contraintuitivo bajo la calibración actual (ver la nota de la Sección 8
  del notebook: reducir el presupuesto nacional a la mitad puede aumentar
  la eficiencia calculada) porque el presupuesto nacional domina el gasto
  total mucho más de lo que impulsa la cobertura agregada; esto es una
  limitación de calibración, no del diseño del modelo, y debe corregirse
  cuando se reemplacen los supuestos por datos reales.
- La métrica M2 (tiempo promedio de respuesta) tenía un defecto de diseño
  ya corregido en el notebook (medía la primera entrega mayor a cero en
  vez de una cobertura mínima significativa); el umbral de cobertura
  mínima que la reemplaza sigue siendo un supuesto calibrado
  empíricamente, no un dato del Excel.
- Proponer cómo se resolverían estas limitaciones con más tiempo o datos
  (ej. datos reales de necesidad per cápita, el mapa de accesibilidad del
  Grupo 4 en lugar de una probabilidad supuesta).
