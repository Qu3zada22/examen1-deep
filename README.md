# Grupo 7 - Política Pública y Asignación de Recursos

CC2017 Modelación y Simulación - Escenario de terremoto en Ciudad UVG.

El Grupo 7 representa la unidad técnica del Comité de Emergencia responsable
de asignar recursos públicos (presupuesto, vehículos pesados, vehículos de
distribución, generadores, combustible, kits de agua, tiendas de campaña)
entre las 5 zonas urbanas de Ciudad UVG (Z1..Z5) a lo largo del horizonte de
72 horas posteriores al evento (12 bloques de 6 horas cada uno), y de evaluar
esa asignación contra cuatro métricas de política pública.

## Estructura del proyecto

```
.
├── pyproject.toml           manifiesto de dependencias gestionado con uv (proyecto no empaquetado)
├── uv.lock                  versiones de dependencias fijadas (se versiona)
├── notebooks/
│   └── Grupo7_Modelo.ipynb  EL entregable unico: todo el modelo vive aqui
├── docs/
│   ├── Grupo7_PoliticaPublica.xlsx   archivo de datos del grupo (fuente de verdad)
│   ├── S10_Examen_Practico.md        transcripcion en markdown del enunciado del examen
│   ├── reporte_estructura.md         esqueleto del reporte (espanol)
│   ├── video_guion.md                guion del video (espanol)
│   ├── g2_conversion_notes.md        notas de conversion de los archivos recibidos de Grupo 2 a CSV
│   ├── g3_conversion_notes.md        notas de conversion del Excel recibido de Grupo 3 a CSV
│   ├── g5_conversion_notes.md        notas de conversion del Excel recibido de Grupo 5 a CSV
│   └── prompts_ia.md                 registro de prompts de IA generativa (espanol, fuente unica)
├── data/
│   ├── raw/                  colocar aqui cualquier archivo Excel adicional o actualizado
│   └── exchange/              reportes CSV recibidos de los Grupos 2, 3 y 5
└── outputs/                   figuras y tablas de resultados exportadas
```

No existe un paquete `src/` ni scripts auxiliares: el entregable de la
tarea es el notebook en sí, por lo que cada función, dataclass y celda vive
directamente en `notebooks/Grupo7_Modelo.ipynb`, en el orden que exige la
narrativa del reporte y del video (ver el índice de secciones en la Sección
0 del notebook).

## Configuración

Este proyecto usa [`uv`](https://docs.astral.sh/uv/) para la gestión de
dependencias (no usa `requirements.txt`: todas las dependencias viven en
`pyproject.toml`).

```bash
uv sync
uv run jupyter lab
```

Luego abrir `notebooks/Grupo7_Modelo.ipynb` y ejecutar todas las celdas de
arriba hacia abajo.

Para volver a ejecutar el notebook completo sin interfaz (por ejemplo, para
regenerar `outputs/` tras un cambio), ejecutar en su lugar:

```bash
uv run jupyter nbconvert --to notebook --execute --inplace notebooks/Grupo7_Modelo.ipynb
```

## Estado de los datos

El archivo Excel del Grupo 7 (`docs/Grupo7_PoliticaPublica.xlsx`) ya llegó,
y es la única fuente de verdad de los recursos disponibles, sus costos
unitarios, la tabla de zonas (daño, población en riesgo, prioridad
declarada) y la definición de las cuatro métricas de evaluación: la Sección
3 del notebook lo lee directamente con `cargar_datos()` (sin hardcodear
ningún valor). Las constantes que antes estaban codificadas quedan ahora
como respaldo (`HARDCODED_*`), útiles solo si el archivo no está disponible,
y `verificar_contra_excel()` compara lo parseado contra ellas para avisar si
llegan a diferir.

**Los tres reportes de intercambio ya llegaron y están convertidos**:
`data/exchange/g2_hospital.csv`, `data/exchange/g3_supplies.csv` y
`data/exchange/g5_personnel.csv` (ver `docs/g2_conversion_notes.md`,
`docs/g3_conversion_notes.md` y `docs/g5_conversion_notes.md` para el
detalle completo de cada conversión, incluyendo los supuestos documentados).
`load_exchange_or_placeholder()` ya detecta los tres archivos y usa los
datos reales — el notebook fue reejecutado de punta a punta con ellos, sin
errores, y ya no muestra el mensaje de placeholder sintético.

### Ya no falta ningún dato de intercambio

Los pasos que siguen ahora son de redacción, no de datos:

1. Revisar/ajustar con criterio propio los factores de conversión
   `# ASSUMPTION:` de `Exchange.need_adjustment` (Sección 4.3 del notebook)
   ahora que los tres CSV son reales, no placeholders.
2. Volver a leer las celdas de la Pregunta 2 (antes/después, Sección 7) y
   de análisis de sensibilidad (Sección 8) con los números reales ya
   recalculados, y escribir las respuestas `[PENDIENTE - equipo]`.
3. Actualizar la Sección 4 ("Incorporación del intercambio presencial") de
   `docs/reporte_estructura.md` con los hallazgos reales de antes/después.

### Si en cambio llega un archivo Excel nuevo o actualizado

1. Colocarlo en `data/raw/`.
2. `uv sync`.
3. `uv run jupyter lab` y ejecutar `describe_workbook()` (Sección 3) contra
   la nueva ruta para inspeccionar su estructura.
4. Si la estructura de secciones cambió, actualizar `cargar_datos()` y las
   constantes `HARDCODED_*` de respaldo en el notebook según el esquema
   recién impreso.
5. Volver a ejecutar todas las celdas.

## Flujo de trabajo antes del intercambio / después del intercambio

- Antes del intercambio: el notebook construye `Params` únicamente a partir
  del archivo Excel del Grupo 7 (pools de recursos, daño/vulnerabilidad por
  zona, prioridades declaradas) y produce una propuesta inicial de
  asignación para 24 horas (Pregunta 1, Sección 5), evaluada contra las
  cuatro métricas sin entradas externas.
- Durante el intercambio: el Grupo 7 debe entregar las plantillas CSV
  descritas en `data/exchange/README.md` a los Grupos 2, 3 y 5 para que sus
  reportes vuelvan exactamente en la forma que el notebook espera (Sección
  6 del notebook documenta qué se recibió).
- Después del intercambio: el notebook se recarga con el paquete
  `Exchange` (saturación hospitalaria del Grupo 2, cuellos de botella de
  suministros del Grupo 3, necesidades de personal/refuerzo del Grupo 5),
  vuelve a ejecutar la simulación de Monte Carlo y produce la tabla
  comparativa antes/después con deltas e intervalos de confianza del 95%
  (Pregunta 2, Sección 7), además de un análisis de sensibilidad sobre el
  presupuesto nacional y sobre la accesibilidad de Z1 (Sección 8).

## Salida final requerida (entregable del examen)

Según la sección 6 de `docs/Grupo7_PoliticaPublica.xlsx`, el Grupo 7 entrega
directamente al profesor (no a otro grupo). La Sección 9 del notebook
produce las cuatro partes y las exporta a `outputs/`:

- (a) un plan de asignación de recursos por zona a 72 horas (`a_allocation_plan_72h.csv`)
- (b) un registro de auditoría cuantitativo que justifica cada decisión de asignación (`b_audit_trail.csv`)
- (c) una evaluación del plan contra las cuatro métricas, con pass/fail e IC del 95% (`c_evaluation_vs_thresholds.csv`)
- (d) un análisis de sensibilidad sobre un presupuesto nacional reducido a la mitad y sobre Z1 inaccesible durante las primeras 12 horas (`d_sensitivity_*.csv`)

## Paradigma

Dinámica de Sistemas (stock-and-flow agregado de recursos, necesidad
insatisfecha y backlog por zona) combinada con una capa programada de
decisión/control (la política de asignación se reevalúa cada bloque de 6
horas). Ver la Sección 1 del notebook para la nota de referencia técnica y
la sección pendiente donde el equipo debe escribir la justificación final
con los criterios vistos en clase.

## Qué falta que escriba el equipo

El notebook (`notebooks/Grupo7_Modelo.ipynb`) marca explícitamente con
`[PENDIENTE - equipo]` cada sección de redacción que todavía falta, para
que no se confunda con el código ya funcional. Faltan, en este orden:

1. Sección 1: justificación del paradigma (con los criterios de clase).
2. Sección 2: descripción del modelo en formato ODD simplificado.
3. Sección 5: respuesta a la Pregunta 1 (cuál métrica es la más difícil de
   cumplir y por qué).
4. ~~Sección 6: completar `intercambio_grupo2`, `intercambio_grupo3` e
   `intercambio_grupo5`~~ — ya está hecho, los tres son reales.
5. Sección 7: respuesta a la Pregunta 2 (qué decisión cambió más y su
   impacto cuantificado).
6. Sección 8: lectura del análisis de sensibilidad (qué decisión cambiaría
   en cada escenario).
7. Sección 10: limitaciones y propuestas de mejora (al menos dos,
   argumentadas por el equipo).
8. Sección 11 y `docs/prompts_ia.md`: documentación de cada uso real de IA
   generativa (prompt, por qué funcionó, qué se ajustó).
9. Adicional: justificar o ajustar los pesos 0.4 / 0.4 / 0.2 de
   `score_prioridad` (Sección 4.2, marcados `# ASSUMPTION:`).

Ninguna de estas secciones debe completarse copiando texto generado por IA
sin razonamiento propio; ver `docs/prompts_ia.md` para el criterio exacto.
