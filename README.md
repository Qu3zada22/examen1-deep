# Grupo 7 - Politica Publica y Asignacion de Recursos

CC2017 Modelacion y Simulacion - Escenario de terremoto en Ciudad UVG.

El Grupo 7 representa la unidad tecnica del Comite de Emergencia responsable
de asignar recursos publicos (presupuesto, vehiculos pesados, vehiculos de
distribucion, generadores, combustible, kits de agua, tiendas de campana)
entre las 5 zonas urbanas de Ciudad UVG (Z1..Z5) a lo largo del horizonte de
72 horas posteriores al evento (12 bloques de 6 horas cada uno), y de evaluar
esa asignacion contra cuatro metricas de politica publica.

## Estructura del proyecto

```
.
├── pyproject.toml           manifiesto de dependencias gestionado con uv (proyecto no empaquetado)
├── uv.lock                  versiones de dependencias fijadas (se versiona)
├── notebooks/
│   └── main.ipynb           EL entregable: todo el modelo vive aqui
├── docs/
│   ├── Grupo7_PoliticaPublica.xlsx   archivo de datos del grupo (fuente de verdad)
│   ├── reporte_estructura.md         esqueleto del reporte (espanol)
│   ├── video_guion.md                guion del video (espanol)
│   └── prompts_ia.md                 registro de prompts de IA generativa (espanol)
├── data/
│   ├── raw/                  colocar aqui cualquier archivo Excel adicional o actualizado
│   └── exchange/              reportes CSV recibidos de los Grupos 2, 3 y 5
└── outputs/                   figuras y tablas de resultados exportadas
```

No existe un paquete `src/` ni scripts auxiliares: el entregable de la
tarea es el notebook en si, por lo que cada funcion, dataclass y celda vive
directamente en `notebooks/main.ipynb`, en el orden que exige la narrativa
del reporte y del video.

## Configuracion

Este proyecto usa [`uv`](https://docs.astral.sh/uv/) para la gestion de
dependencias.

```bash
uv sync
uv run jupyter lab
```

Luego abrir `notebooks/main.ipynb` y ejecutar todas las celdas de arriba
hacia abajo.

Para volver a ejecutar el notebook completo sin interfaz (por ejemplo, para
regenerar `outputs/` tras un cambio), ejecutar en su lugar:

```bash
uv run jupyter nbconvert --to notebook --execute --inplace notebooks/main.ipynb
```

## Estado de los datos

El archivo Excel del Grupo 7 (`docs/Grupo7_PoliticaPublica.xlsx`) ya llego,
y sus pools de recursos, tabla de dano por zona y restricciones de politica
estan codificados directamente en las celdas `Params` y de verificacion de
restricciones del notebook. El notebook tambien incluye una celda de
parseo que vuelve a leer el archivo directamente y cruza las constantes
codificadas contra el, de modo que el notebook sigue demostrando que lee el
archivo correctamente y sigue funcionando de principio a fin si el archivo
llega a moverse o no estar disponible.

Los tres reportes de intercambio (de los Grupos 2, 3 y 5) todavia no
existen, se produciran unicamente durante el intercambio presencial. Hasta
entonces, la seccion de intercambio del notebook usa como respaldo un
pequeno intercambio sintetico de referencia para que el flujo "antes/despues"
(Pregunta 2) pueda ejecutarse desde ya.

### Cuando lleguen los datos del intercambio

1. Colocar los tres archivos CSV (siguiendo los esquemas de
   `data/exchange/README.md`) en `data/exchange/`:
   `g2_hospital.csv`, `g3_supplies.csv`, `g5_personnel.csv`.
2. `uv sync` (solo si cambiaron las dependencias).
3. `uv run jupyter lab` y volver a ejecutar la celda de carga del
   intercambio del notebook: prefiere automaticamente los archivos reales
   sobre el placeholder sintetico en cuanto estan presentes.
4. Volver a ejecutar las celdas de la Pregunta 2 (antes/despues) y de
   analisis de sensibilidad.
5. Actualizar la seccion "Incorporacion del intercambio" de
   `docs/reporte_estructura.md` con los hallazgos reales de antes/despues.

### Si en cambio llega un archivo Excel nuevo o actualizado

1. Colocarlo en `data/raw/`.
2. `uv sync`.
3. `uv run jupyter lab` y ejecutar la celda de parseo del Excel contra la
   nueva ruta.
4. Actualizar los valores por defecto de `Params` y las constantes de
   restricciones en el notebook segun el esquema recien impreso.
5. Volver a ejecutar todas las celdas.

## Flujo de trabajo antes del intercambio / despues del intercambio

- Antes del intercambio: el notebook construye `Params` unicamente a partir
  del archivo Excel del Grupo 7 (pools de recursos, dano/vulnerabilidad por
  zona, prioridades declaradas) y produce una propuesta inicial de
  asignacion para 24 horas (Pregunta 1), evaluada contra las cuatro
  metricas sin entradas externas.
- Durante el intercambio: el Grupo 7 debe entregar las plantillas CSV
  descritas en `data/exchange/README.md` a los Grupos 2, 3 y 5 para que sus
  reportes vuelvan exactamente en la forma que el notebook espera.
- Despues del intercambio: el notebook se recarga con el paquete
  `Exchange` (saturacion hospitalaria del Grupo 2, cuellos de botella de
  suministros del Grupo 3, necesidades de personal/refuerzo del Grupo 5),
  vuelve a ejecutar la simulacion de Monte Carlo y produce la tabla
  comparativa antes/despues con deltas e intervalos de confianza del 95%
  (Pregunta 2), ademas de un analisis de sensibilidad sobre el presupuesto
  nacional y sobre la accesibilidad de Z1.

## Salida final requerida (entregable del examen)

Segun la seccion 6 de `docs/Grupo7_PoliticaPublica.xlsx`, el Grupo 7 entrega
directamente al profesor (no a otro grupo). La seccion final del notebook
produce las cuatro partes y las exporta a `outputs/`:

- (a) un plan de asignacion de recursos por zona a 72 horas (`a_allocation_plan_72h.csv`)
- (b) un registro de auditoria cuantitativo que justifica cada decision de asignacion (`b_audit_trail.csv`)
- (c) una evaluacion del plan contra las cuatro metricas, con pass/fail e IC del 95% (`c_evaluation_vs_thresholds.csv`)
- (d) un analisis de sensibilidad sobre un presupuesto nacional reducido a la mitad y sobre Z1 inaccesible durante las primeras 12 horas (`d_sensitivity_*.csv`)

## Paradigma

Dinamica de Sistemas (stock-and-flow agregado de recursos, necesidad
insatisfecha y backlog por zona) combinada con una capa programada de
decision/control (la politica de asignacion se reevalua cada bloque de 6
horas). Esto explicitamente no es un modelo basado en agentes (ningun
tomador de decisiones individual es el objeto de estudio) ni una simulacion
de eventos discretos (no hay disciplina de colas de entidades discretas).
Ver la celda markdown de justificacion del paradigma en el notebook para el
argumento completo.
