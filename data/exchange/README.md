# Datos de intercambio - esquemas esperados

El Grupo 7 RECIBE tres reportes en el intercambio presencial. Es necesario
entregar estas plantillas CSV exactas a los Grupos 2, 3 y 5 en el
intercambio, para que sus datos lleguen en la forma que el notebook espera,
en lugar de tener que ser reconstruidos despues por ingenieria inversa.

Colocar aqui los tres archivos con estos nombres exactos una vez recibidos:

- `g2_hospital.csv`
- `g3_supplies.csv`
- `g5_personnel.csv`

## Del Grupo 2: saturacion hospitalaria + recursos criticos

Archivo: `g2_hospital.csv`

| columna | tipo | descripcion |
|---|---|---|
| `zone` | cadena | Uno de `Z1`..`Z5` |
| `block` | entero | 1..12 (indice de bloque de 6h) |
| `saturation_index` | flotante | Indice de saturacion hospitalaria para esta zona/bloque (0-1 o segun lo definido por el Grupo 2) |
| `critical_resource` | cadena | Nombre del recurso critico en escasez (por ejemplo, camas, personal medico, insumos medicos) |
| `deficit_units` | flotante | Cantidad faltante del recurso critico |

Fila de ejemplo:

```csv
zone,block,saturation_index,critical_resource,deficit_units
Z1,1,0.95,camas,40.0
```

## Del Grupo 3: balance de suministros + cuellos de botella

Archivo: `g3_supplies.csv`

| columna | tipo | descripcion |
|---|---|---|
| `zone` | cadena | Uno de `Z1`..`Z5` |
| `block` | entero | 1..12 |
| `supply_type` | cadena | Categoria de suministro (por ejemplo, agua, alimentos) |
| `balance_units` | flotante | Balance neto (negativo = deficit) |
| `bottleneck_flag` | booleano | Indica si esta zona/bloque/suministro es un cuello de botella reportado |

Fila de ejemplo:

```csv
zone,block,supply_type,balance_units,bottleneck_flag
Z1,1,agua,-100.0,True
```

## Del Grupo 5: plan de despliegue de personal + requerimientos de refuerzo

Archivo: `g5_personnel.csv`

| columna | tipo | descripcion |
|---|---|---|
| `zone` | cadena | Uno de `Z1`..`Z5` |
| `block` | entero | 1..12 |
| `personnel_assigned` | entero | Personal actualmente asignado a esta zona/bloque |
| `reinforcement_needed` | entero | Personal adicional necesario |

Fila de ejemplo:

```csv
zone,block,personnel_assigned,reinforcement_needed
Z1,1,20,10
```

## Notas

- `zone` debe usar exactamente `Z1`..`Z5` y `block` debe ser un entero en
  `[1, 12]`; los cargadores del notebook (`load_g2_hospital`,
  `load_g3_supplies`, `load_g5_personnel`) validan ambos dominios y lanzan
  un mensaje de error accionable en caso contrario.
- Mientras estos archivos no existan, `notebooks/main.ipynb` usa como
  respaldo un pequeno intercambio sintetico de referencia (claramente
  etiquetado en la salida del notebook) para que el flujo antes/despues
  (Pregunta 2) y el analisis de sensibilidad puedan ejecutarse de principio
  a fin.
- La conversion exacta de las columnas de cada reporte en un ajuste de
  necesidad/capacidad dentro del modelo esta implementada en
  `Exchange.need_adjustment` en el notebook (Seccion 6), y alli se
  documenta con factores de conversion etiquetados `# ASSUMPTION:` que el
  grupo debe revisar en cuanto lleguen los datos reales.
