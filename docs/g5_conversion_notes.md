# Conversión de `Grupo5_Plan_Despliegue.xlsx` a `g5_personnel.csv`

El archivo recibido de Grupo 5 (`Grupo5_Plan_Despliegue.xlsx`, 5 hojas) es mucho
más rico que el schema `zone, block, personnel_assigned, reinforcement_needed`
que espera el notebook (ver `data/exchange/README.md`). Esta nota documenta
cómo se hizo la conversión y qué se perdió/asumió en el camino, para que el
equipo pueda citarlo en la sección "Incorporación del intercambio" del reporte.

## Mapeo de bloques

`T0..T11` (columnas del Excel de Grupo 5) → `block 1..12` (T0 = bloque 1,
T11 = bloque 12). Igual convención de 6h que el resto del examen.

## Z1 y Z5: pool de personal combinado (supuesto de conversión)

El Excel de Grupo 5 reporta a Z1 y Z5 como una sola categoría
("Z1 y Z5 (Críticas)") en la hoja "2. Plan de Despliegue" — no separa
personal por zona individual. Como el schema de Grupo 7 es por zona, se
dividió el total 50/50 entre Z1 y Z5 en cada bloque (residuo de redondeo
asignado a Z5, sin ningún criterio adicional — es un desempate arbitrario).

**`personnel_assigned`** = suma de todos los tipos de personal en esa
categoría combinada (bomberos originales + bomberos departamentales +
ejército ingeniería original + 2do batallón + PNC), hoja 2.

**`reinforcement_needed`** = el déficit exacto que reporta la hoja
"3. Rescate Z1-Z5" en su conclusión (Output B): **3 personas faltan para
completar el equipo 12 de rescate**, únicamente en los bloques 5 y 6 (T4-T5,
24-36h). 0 en el resto de bloques. Ese déficit de 3 se dividió también
50/50 entre Z1 y Z5 (2 y 1) por el mismo motivo — Grupo 5 no distingue cuál
de las dos zonas concentra el déficit.

## Z3, Z2, Z4

Estas sí vienen desagregadas por zona en la hoja 2, sin ambigüedad:

- **Z3**: Ejército ingeniería (original + 2do batallón) + Voluntarios CONRED
  (apoyo) + PNC.
- **Z2** y **Z4**: Cruz Roja + Voluntarios CONRED (apoyo) + PNC — constante
  (63 personas) en los 12 bloques.

Grupo 5 no reportó ningún déficit/alerta para estas tres zonas, así que
`reinforcement_needed = 0` en todos sus bloques.

## Inconsistencia de datos detectada (no corregida silenciosamente)

La hoja "3. Rescate Z1-Z5" reporta `BOMB.REFUERZO = 45` en **T0 (bloque 1)**,
pero:
- su propia nota en esa fila dice *"Bomberos originales: turno 1 de 2"*,
  implicando que el refuerzo aún no debería haber llegado, y
- la hoja 2 marca explícitamente "—" (ausente) para "Bomberos
  departamentales" en T0, subiendo recién a 22 en T1.

Se interpreta como un error de captura en el archivo de Grupo 5, y **no se
corrigió** en la fuente — simplemente no se usó ese dato: `personnel_assigned`
para Z1/Z5 en el bloque 1 se calculó con la hoja 2 (que sí marca 0 en T0), y
`reinforcement_needed` no se ve afectado porque el déficit reportado por
Grupo 5 solo aplica a los bloques 5-6, no al bloque 1.

## Información recibida que NO entra en el CSV (fuera del schema)

Estas partes del Excel de Grupo 5 son reales y útiles, pero no encajan en
`zone/block/personnel_assigned/reinforcement_needed` y **no se modelan
numéricamente** — mencionarlas cualitativamente si hace falta en el reporte:

- **Albergues** y **Hospitales/Clínicas** (hoja 2): categorías
  institucionales, no zonas geográficas Z1-Z5.
- **EPP (equipo de protección personal):** solo 80 sets en T0, límite
  absoluto de personas que pueden operar en Z1/Z5 simultáneamente. La
  brecha de 3 personas en los bloques 5-6 es, en la práctica, una brecha de
  EPP disponible más que de personal disponible (hoja 4B: "4 militares...
  necesitan EPP para cubrir el equipo 12").
- **Combustible:** 35 vehículos, 1,400 gal de reserva, autonomía de 5 días
  (120h) — cubre el horizonte de 72h sin problema.
- **Ratio de coordinadores CONRED:** 1 por cada 25 voluntarios no
  entrenados → mínimo 20 coordinadores desde T0 en albergues.
- **Dependencia declarada de Grupo 4:** Grupo 5 asumió accesibilidad total
  a todas las zonas desde T0 — su propio archivo advierte que el mapa de
  aislamiento de Grupo 4 "PUEDE modificar el despliegue en Z2/Z4".

## Cómo se generó el archivo

Script de conversión (no versionado, vive en el scratchpad de la sesión que
lo generó) escribe directamente `data/exchange/g5_personnel.csv` a partir de
los números citados arriba, y valida columnas/dominio de zona/dominio de
bloque/conteo de filas (5 zonas × 12 bloques = 60) antes de guardar.
