# Conversión de `grupo3_entrega_grupo7.xlsx` a `g3_supplies.csv`

Grupo 3 entregó un Excel de 15 hojas (`data/exchange/grupo3_entrega_grupo7.xlsx`,
carpeta ignorada por git, igual que los archivos de Grupo 2 y Grupo 5), mucho
más rico que el esquema `zone, block, supply_type, balance_units,
bottleneck_flag` que espera el notebook (ver `data/exchange/README.md`).
Esta nota documenta la conversión, siguiendo el mismo patrón que
[`g2_conversion_notes.md`](g2_conversion_notes.md) y
[`g5_conversion_notes.md`](g5_conversion_notes.md).

A diferencia de Grupo 2 y Grupo 5, esta conversión **requirió muy pocos
supuestos**: Grupo 3 ya entrega una hoja (`o01_balance_resumen_intercambio`)
exactamente a la granularidad zona × bloque × suministro que necesita el
esquema, con `deficit_medio` y `superavit_medio` ya calculados sobre 100
realizaciones de Monte Carlo.

## Qué escenario se usó

El Excel trae **dos escenarios** en paralelo: `base_pre_intercambio` (solo
datos propios de Grupo 3) y `post_intercambio` (después de incorporar los
reportes de Grupo 1 y Grupo 4). Se usó **`post_intercambio`** para
`g3_supplies.csv`, porque es el que corresponde a lo que Grupo 3 le entrega
formalmente a Grupo 7 según la cadena de dependencia del examen (Grupo 3 → Grupo 7:
"Balance de suministros y cuellos de botella"), y porque ya incorpora la
población real de Grupo 1 (ver más abajo).

## Mapeo de columnas (sin ambigüedad)

De la hoja `o01_balance_resumen_intercambio`, para cada fila
zona/bloque/suministro:

- **`zone`**: columna `zona`, directo (Z1-Z5, sin necesidad de inferir nada:
  Grupo 3 ya reporta por zona, no por instalación).
- **`block`**: columna `bloque` (0-11, es decir T0-T11) **+ 1**, para usar la
  misma convención 1-12 del resto del examen.
- **`supply_type`**: columna `suministro` (`agua`, `alimentos`,
  `medicamentos`), directo.
- **`balance_units`** = `superavit_medio − deficit_medio` de esa fila. En cada
  fila exactamente uno de los dos es distinto de cero (el modelo de Grupo 3
  nunca tiene superávit y déficit simultáneos), así que este balance neto es
  negativo exactamente cuando hay déficit y positivo cuando hay inventario
  sobrante — coincide con la definición del esquema ("Balance neto, negativo
  = déficit") sin necesidad de ningún supuesto adicional.
- **`bottleneck_flag`** = `deficit_medio > 0` en esa fila. También sin
  supuestos: es el dato real de si esa zona/bloque/suministro tuvo demanda
  no satisfecha en el escenario post-intercambio.

Resultado: 180 filas (5 zonas × 12 bloques × 3 suministros), sin huecos.
48 filas quedan con `bottleneck_flag=True`: las 36 de Z1 (todo el horizonte,
los 3 suministros) más 6 de Z3 y 6 de Z5 (solo medicamentos, bloques 6-11 en
convención 0-index / 7-12 en la convención de Grupo 7) — coincide
exactamente con lo que Grupo 3 reporta en su propia hoja
`cuellos_de_botella` y `o02_deficit_por_zona_intercambi`.

## Por qué Z1 tiene déficit en el 100% de los bloques (hallazgo estructural, no un error)

Grupo 3 documenta explícitamente (hoja `cobertura_zonas` y
`cuellos_de_botella`) que **Z1 no tiene centro de acopio ni ninguna ruta de
entrada en la red de su Excel original**, pero Grupo 1 sí le reporta
población/demanda a Z1. El resultado es que el 100% del consumo de Z1 queda
como déficit desde el bloque 1, no por agotamiento de inventario sino por
ausencia total de vía de abastecimiento — un cuello de botella **estructural**
de la red, no operativo. Grupo 3 decidió documentar la brecha en vez de
inventar una ruta que no está en su archivo fuente (ver hoja
`supuestos_intercambio`, fila "Z1 sin población estimable...").

Z3 tampoco tiene convoy externo posible (todas sus rutas salen de su propio
centro), así que si su inventario local se agota, no se puede reponer dentro
del horizonte — de ahí su déficit de medicamentos en los últimos bloques.

## Información recibida que NO entra en el CSV (fuera del schema, pero relevante para el reporte)

- **Corrección de población (Grupo 1)**: el supuesto propio de Grupo 3 (k=4.5
  personas/m³ de capacidad de almacenamiento) estimaba 19,125 personas/bloque;
  la proyección real de Grupo 1 fue de 8,359/bloque — una reducción del
  **-56.3%** en población, y por consiguiente en toda la demanda derivada
  (tabla `supuestos_intercambio`, fila 1). Este es exactamente el tipo de
  contradicción que la Sección 4 del examen pide documentar explícitamente.
- **Restricciones de vehículo (Grupo 4)**: en varias rutas (R02, R03, R04,
  R05, R07, R08, R09) solo puede circular vehículo liviano en ciertos
  bloques, lo que baja la capacidad de transporte respecto al camión de 10 t.
- **Rutas Grupo 4 no actualizadas**: R08 y R09 (hacia albergues Z5-1 y Z5-2)
  siguen sin habilitarse formalmente por Grupo 4.
- **Tiempos de tránsito reales desde Z4** (Bodega Central CONRED):
  sustituyeron el supuesto de "rutas dañadas al 40% de velocidad" (4.26h)
  por el tiempo mediano observado de Grupo 4 (0.50h) en las rutas que salen
  de ahí — llegadas más tempranas, mayor margen de activación del convoy.
  Ver hoja `o03_convoy_margen_intervencion` para el detalle antes/después.
- **Índice de aislamiento de Grupo 4**: se anexó como descriptor de
  contexto por zona/bloque, pero Grupo 3 no lo usa para derivar tiempos ni
  capacidades (no hay fórmula declarada que lo vincule).
- **Comparación agregada de escenarios** (hoja `comparacion_escenarios`):
  la demanda total baja de 231,905 a 101,456 por la corrección de población.
  El déficit en **zonas abastecibles** (Z2-Z5) **cae** de 509.3 a 231.0
  gracias a esa misma corrección (menos población real que la supuesta). Pero
  el déficit **total** (incluyendo Z1) **sube** de 509.3 a 5,119.3, porque el
  escenario base nunca le asignó población a Z1 ("Z1 sin población estimable
  por no tener centro de acopio"), así que su déficit estructural era
  invisible antes del intercambio y solo aparece al incorporar la proyección
  real de Grupo 1. Vale la pena citar ambos números en la Sección 7 del
  reporte para no confundir "más déficit total" con "peor desempeño real":
  el sistema realmente sirviéndose (Z2-Z5) mejoró, y lo que subió es una
  brecha estructural de Z1 que el modelo base simplemente no veía.

## Nota sobre cómo lo consume `Exchange.need_adjustment`

El código de la Sección 4.3 del notebook (ya existente, sin cambios) suma
**todo** `balance_units < 0` de `g3_supplies.csv` para una zona/bloque —
agua, alimentos y medicamentos juntos — y lo traduce 1 a 1 en necesidad
adicional de `kits_agua` (el único recurso "de agua" en el pool de Grupo 7).
Esto ya estaba marcado `# ASSUMPTION` antes de esta conversión y sigue
siéndolo: mezclar déficits de alimentos/medicamentos (magnitud ~5-100) con
los de agua (magnitud ~300) dentro de una sola necesidad de kits de agua es
una simplificación preexistente del modelo de Grupo 7, no algo introducido
por esta conversión. Queda como punto a revisar en la Sección 10
(Limitaciones) si el equipo quiere separar los tres suministros en
necesidades distintas.

## Cómo se generó el archivo

Script de conversión: `build_g3_csv.py` (vive en el scratchpad de la sesión
que lo generó — igual que g2 y g5, si se pierde debe regenerarse leyendo
la hoja `o01_balance_resumen_intercambio` del Excel de Grupo 3 con la lógica
de arriba). Valida columnas/dominio de zona/dominio de bloque/conteo de
filas (180) antes de guardar `data/exchange/g3_supplies.csv`.
