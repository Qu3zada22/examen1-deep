# Conversión de los archivos de Grupo 2 a `g2_hospital.csv`

Grupo 2 entregó cuatro archivos (`REPORTE-GRUPO7.md`,
`saturacion_por_bloque.csv`, `cuellos_botella.csv`, `recursos_minimos.csv`),
guardados en `data/exchange/` (carpeta ignorada por git, igual que el Excel
de Grupo 5), mucho más ricos que el esquema `zone, block, saturation_index,
critical_resource, deficit_units` que espera el notebook (ver
`data/exchange/README.md`). Esta nota documenta cómo se hizo la conversión y
qué se perdió/asumió en el camino, para citarla en la sección "Incorporación
del intercambio" del reporte (mismo patrón que
[`g5_conversion_notes.md`](g5_conversion_notes.md) para Grupo 5).

## Mapeo instalación → zona (ASSUMPTION)

Grupo 2 reporta por **instalación**, no por zona Z1-Z5. Solo dos nombres
traen la zona explícita:

| Instalación | Zona | Base de la asignación |
|---|---|---|
| Clínica Z2 | Z2 | Explícito en el nombre |
| Puesto Salud Z5 | Z5 | Explícito en el nombre |
| Hospital General UVG | **Z1** | **ASSUMPTION**: se infiere por rol (hospital general/central) y por ser Z1 la zona "Centro histórico" en el archivo de Grupo 7 |
| Hospital Regional Este | **Z4** | **ASSUMPTION**: se infiere por el nombre ("Este") y por ser Z4 la zona "Este comercial" en el archivo de Grupo 7 |
| Centro Salud Z3 | Z3 | Sin datos: el reporte declara explícitamente que "el Centro Salud Z3 está cerrado y no recibe pacientes, por lo que no aparece" |

Grupo 2 nunca declaró este mapeo de forma explícita; **si el equipo confirma
con Grupo 2 que la asignación de Hospital General UVG o Hospital Regional
Este es distinta, hay que regenerar el CSV** (ver script de conversión más
abajo). Z3 queda sin filas en `g2_hospital.csv` (no un error de carga: el
loader del notebook no exige cobertura completa de zona×bloque, solo que las
zonas/bloques presentes estén en el dominio válido).

## Fuentes por columna

- **`saturation_index`** y **`critical_resource`**: para cada zona/bloque se
  toma el **máximo** de la ocupación entre los 4 recursos hospitalarios
  (personal médico, quirófanos, camas UCI, camas generales) — ese máximo es
  el recurso que realmente restringe el sistema en ese momento. Camas
  generales y UCI vienen de `saturacion_por_bloque.csv` (columna
  `ocupacion_media_pct`); **quirófano y personal médico NO están en ese CSV**
  (el propio reporte aclara: "el intervalo celda por celda de camas y UCI
  está en `saturacion_por_bloque.csv`") y se transcribieron directamente de
  la tabla de la Sección 1 de `REPORTE-GRUPO7.md` (medias de 30 réplicas
  Monte Carlo, redondeadas a enteros en la tabla). Desempate en caso de
  empate exacto: médico > quirófano > cama UCI > cama general, siguiendo el
  propio orden de criticidad que usa Grupo 2 en `cuellos_botella.csv`.
- **`deficit_units`** (ASSUMPTION): el esquema del notebook lo define como
  "cantidad faltante del recurso crítico" y `Exchange.need_adjustment`
  (Sección 4.3) lo convierte 1 a 1 en necesidad extra de
  `presupuesto_municipal` (a razón de Q500/unidad). Grupo 2 no reporta un
  conteo de unidades faltantes por zona/bloque, así que se construyó así:
  - Si el recurso crítico de esa fila es **personal médico**:
    `deficit_units = max(0, ocupación_médico% − 80)`, es decir, puntos
    porcentuales de ocupación por encima de un umbral seguro de 80% (mismo
    umbral que usa la métrica M3 del propio modelo de Grupo 7).
  - Si el recurso crítico es **quirófano** (o, en teoría, camas):
    `deficit_units = 0`. Esto no es un descuido: la Sección 2 de
    `REPORTE-GRUPO7.md` prueba explícitamente que relajar quirófanos un 20%
    da una diferencia en mortalidad "indistinguible de cero" y que camas/UCI
    dan una diferencia de "exactamente cero" — Grupo 2 concluye que
    **personal médico es el único recurso cuyo refuerzo reduce la
    mortalidad**. Traducir un déficit de quirófano en presupuesto municipal
    adicional habría sido injertar una necesidad que el propio análisis de
    Grupo 2 descarta.

## Información recibida que NO entra en el CSV (fuera del schema)

Real y relevante para el reporte, pero no encaja en
`zone/block/saturation_index/critical_resource/deficit_units`:

- **Mortalidad evitable agregada**: 2,787 muertes evitables (IC 2,601-2,973),
  24.5% de tasa (IC 23.2-25.8) — dato directo para responder la Pregunta 2
  de Grupo 7 (Sección 7 del notebook).
  * "+60 médicos" es el único incremento de recurso que Grupo 2 identificó
  como suficiente para bajar la mortalidad evitable de la ciudad completa
  por debajo del umbral de 15% (13.3% resultante); corrido con solo 10
  réplicas sin IC, según su propia nota de limitación.
- Desglose por gravedad (grave/moderado/leve) de quién muere esperando.
- Ventanas de supervivencia en cola supuestas por Grupo 2 (4h graves, 24h
  moderados, 120h leves) y consumo fijo de insumos por procedimiento — sus
  propios supuestos no dados por el archivo fuente.

## Cómo se generó el archivo

Script de conversión: `build_g2_csv.py` (no versionado, vive en el
scratchpad de la sesión que lo generó — igual que ocurrió con el CSV de
Grupo 5, este script debe volver a ejecutarse si el archivo se pierde;
ver la lección aprendida en `g5_conversion_notes.md`). Lee
`saturacion_por_bloque.csv`, combina con las tablas transcritas de
`REPORTE-GRUPO7.md`, aplica el mapeo instalación→zona y las reglas de
`deficit_units` de arriba, y valida columnas/dominio de zona/dominio de
bloque antes de guardar `data/exchange/g2_hospital.csv` (48 filas: 4 zonas
con datos × 12 bloques; Z3 sin filas).
