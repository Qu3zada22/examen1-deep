# Grupo 7 - Política Pública y Asignación de Recursos

CC2017 Modelación y Simulación - Examen Práctico (Semana 10).

## El escenario

El 14 de noviembre a las 4:00 AM, un terremoto de magnitud 6.8 sacude Ciudad
UVG, una ciudad de 250,000 habitantes dividida en cinco zonas urbanas
(Z1..Z5). La Municipalidad activa el Comité de Emergencia, y cada grupo del
curso representa una unidad técnica de ese comité (enunciado completo en
`docs/S10_Examen_Practico.md`).

El Grupo 7 es la unidad de **Política Pública y Asignación de Recursos**:
responsable de asignar recursos públicos (presupuesto municipal y nacional,
vehículos pesados, vehículos de distribución, generadores, combustible,
kits de agua, tiendas de campaña) entre las 5 zonas a lo largo de las 72
horas posteriores al evento (12 bloques de 6 horas), y de evaluar esa
asignación contra cuatro métricas de política pública (mortalidad evitable,
tiempo de respuesta, cobertura de suministros y eficiencia presupuestaria).
A diferencia de los demás grupos, el Grupo 7 no entrega su output a otro
grupo: produce el cierre del examen directamente para el profesor.

## Qué se hizo

Todo el modelo, el análisis y las respuestas del grupo viven en un único
notebook, `notebooks/Grupo7_Modelo.ipynb`:

- **Paradigma**: Dinámica de Sistemas (stock-and-flow agregado de recursos,
  necesidad insatisfecha y backlog por zona) combinada con una capa
  programada de decisión/control — la política de asignación se reevalúa
  cada bloque de 6 horas. Justificado formalmente contra los criterios de
  selección de paradigma vistos en clase (Sección 1 del notebook).
- **Modelo**: descrito en formato ODD simplificado (Sección 2) — 5 zonas
  heterogéneas por daño, población en riesgo y prioridad declarada; 8 tipos
  de recurso con stock y costo propios; 6 políticas de asignación
  comparables entre sí (`equal_share`, `proportional_to_need`,
  `severity_weighted`, `worst_first`, `threshold_then_proportional`,
  `score_prioridad`).
- **Datos**: se leen directamente de `docs/Grupo7_PoliticaPublica.xlsx`
  (`cargar_datos()`, Sección 3), sin hardcodear valores.
- **Incertidumbre**: 300 réplicas de Monte Carlo por escenario, resultados
  reportados como media + intervalo de confianza del 95% (nunca como valor
  puntual).
- **Intercambio presencial**: se integraron los tres reportes recibidos de
  los Grupos 2 (saturación hospitalaria), 3 (balance de suministros) y 5
  (despliegue de personal), vía `Exchange.need_adjustment` (Sección 4.3).
  La Sección 7 responde la Pregunta 2 comparando la asignación y las
  métricas antes/después de esa integración, con deltas e IC del 95%.
- **Preguntas del grupo** (Secciones 5 y 7) y **análisis de sensibilidad**
  (Sección 8, presupuesto nacional a la mitad y Z1 inaccesible las primeras
  12h) respondidos con los números reales del modelo.
- **Salida final requerida por el examen** (Sección 9): plan de asignación
  a 72h, registro de auditoría, evaluación contra las 4 métricas y análisis
  de sensibilidad, exportados a `outputs/`.
- **Limitaciones** del modelo documentadas con propuestas concretas de
  mejora (Sección 10).

## Estructura del proyecto

```
.
├── pyproject.toml           manifiesto de dependencias gestionado con uv
├── uv.lock                  versiones de dependencias fijadas
├── notebooks/
│   └── Grupo7_Modelo.ipynb  el entregable único: todo el modelo vive aquí
├── docs/
│   ├── Grupo7_PoliticaPublica.xlsx   archivo de datos del grupo (fuente de verdad)
│   ├── S10_Examen_Practico.md        enunciado del examen
│   ├── reporte_estructura.md         esqueleto del reporte técnico
│   ├── video_guion.md                borrador de guion para ensayar el video
│   ├── g2_conversion_notes.md        conversión de los reportes de Grupo 2 a CSV
│   ├── g3_conversion_notes.md        conversión de los reportes de Grupo 3 a CSV
│   ├── g5_conversion_notes.md        conversión de los reportes de Grupo 5 a CSV
│   └── prompts_ia.md                 registro de prompts de IA generativa
├── data/
│   ├── raw/                  archivo(s) Excel adicionales o actualizados
│   └── exchange/              reportes CSV recibidos de los Grupos 2, 3 y 5
└── outputs/                   figuras y tablas de resultados exportadas
```

No existe un paquete `src/` ni scripts auxiliares: cada función, dataclass
y celda vive directamente en el notebook, en el orden que exige la
narrativa del reporte y del video (ver el índice de secciones en la
Sección 0 del notebook).

## Cómo correrlo

Este proyecto usa [`uv`](https://docs.astral.sh/uv/) para la gestión de
dependencias (todas viven en `pyproject.toml`, sin `requirements.txt`).

```bash
uv sync
uv run jupyter lab
```

Luego abrir `notebooks/Grupo7_Modelo.ipynb` y ejecutar todas las celdas de
arriba hacia abajo.

Para volver a ejecutar el notebook completo sin interfaz (por ejemplo, para
regenerar `outputs/` tras un cambio):

```bash
uv run jupyter nbconvert --to notebook --execute --inplace notebooks/Grupo7_Modelo.ipynb
```

Los tres CSV de intercambio (`data/exchange/g2_hospital.csv`,
`g3_supplies.csv`, `g5_personnel.csv`) no se versionan en git (ver
`.gitignore`); deben estar presentes en `data/exchange/` para que el
notebook use los datos reales del intercambio en vez de caer al pequeño
intercambio sintético de referencia que trae `load_exchange_or_placeholder()`
como respaldo (Sección 4.3).

Si en cambio llega un archivo Excel nuevo o actualizado del propio Grupo 7:

1. Colocarlo en `data/raw/`.
2. `uv sync`.
3. `uv run jupyter lab` y ejecutar `describe_workbook()` (Sección 3) contra
   la nueva ruta para inspeccionar su estructura.
4. Si la estructura de secciones cambió, actualizar `cargar_datos()` y las
   constantes `HARDCODED_*` de respaldo en el notebook según el esquema
   recién impreso.
5. Volver a ejecutar todas las celdas.

## Salida final requerida (entregable del examen)

Según la sección 6 de `docs/Grupo7_PoliticaPublica.xlsx`, el Grupo 7
entrega directamente al profesor. La Sección 9 del notebook produce las
cuatro partes y las exporta a `outputs/`:

- (a) plan de asignación de recursos por zona a 72 horas (`a_allocation_plan_72h.csv`)
- (b) registro de auditoría cuantitativo que justifica cada decisión de asignación (`b_audit_trail.csv`)
- (c) evaluación del plan contra las cuatro métricas, con pass/fail e IC del 95% (`c_evaluation_vs_thresholds.csv`)
- (d) análisis de sensibilidad sobre un presupuesto nacional reducido a la mitad y sobre Z1 inaccesible durante las primeras 12 horas (`d_sensitivity_*.csv`)

Ninguna sección del notebook o del reporte debe completarse copiando texto
generado por IA sin razonamiento propio; ver `docs/prompts_ia.md` para el
registro de cada prompt usado y por qué funcionó.
