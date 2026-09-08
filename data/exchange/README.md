# Exchange data - expected schemas

Group 7 RECEIVES three reports at the in-person exchange. **Hand these
exact CSV templates to Groups 2, 3, and 5 at the exchange** so their data
arrives in the shape the notebook expects, instead of having to be
reverse-engineered afterwards.

Drop the three files here with these exact names once received:

- `g2_hospital.csv`
- `g3_supplies.csv`
- `g5_personnel.csv`

## From Group 2: hospital saturation + critical resources

File: `g2_hospital.csv`

| column | type | description |
|---|---|---|
| `zone` | string | One of `Z1`..`Z5` |
| `block` | integer | 1..12 (6h block index) |
| `saturation_index` | float | Hospital saturation index for this zone/block (0-1 or as defined by Group 2) |
| `critical_resource` | string | Name of the critical resource in shortage (e.g. beds, medical staff, medical supplies) |
| `deficit_units` | float | Quantity of the critical resource missing |

Example row:

```csv
zone,block,saturation_index,critical_resource,deficit_units
Z1,1,0.95,camas,40.0
```

## From Group 3: supply balance + bottlenecks

File: `g3_supplies.csv`

| column | type | description |
|---|---|---|
| `zone` | string | One of `Z1`..`Z5` |
| `block` | integer | 1..12 |
| `supply_type` | string | Supply category (e.g. water, food) |
| `balance_units` | float | Net balance (negative = deficit) |
| `bottleneck_flag` | boolean | Whether this zone/block/supply is a reported bottleneck |

Example row:

```csv
zone,block,supply_type,balance_units,bottleneck_flag
Z1,1,agua,-100.0,True
```

## From Group 5: personnel deployment plan + reinforcement requirements

File: `g5_personnel.csv`

| column | type | description |
|---|---|---|
| `zone` | string | One of `Z1`..`Z5` |
| `block` | integer | 1..12 |
| `personnel_assigned` | integer | Personnel currently assigned to this zone/block |
| `reinforcement_needed` | integer | Additional personnel needed |

Example row:

```csv
zone,block,personnel_assigned,reinforcement_needed
Z1,1,20,10
```

## Notes

- `zone` must use exactly `Z1`..`Z5` and `block` must be an integer in
  `[1, 12]`; the notebook's loaders (`load_g2_hospital`, `load_g3_supplies`,
  `load_g5_personnel`) validate both domains and raise an actionable error
  message otherwise.
- Until these files exist, `notebooks/main.ipynb` falls back to a small
  synthetic placeholder exchange (clearly labeled in the notebook output)
  so the before/after pipeline (Question 2) and the sensitivity analysis
  can still run end to end.
- The exact conversion from each report's columns into a need/capacity
  adjustment inside the model is implemented in `Exchange.need_adjustment`
  in the notebook (Section 6) and is documented there with
  `# ASSUMPTION:`-tagged conversion factors that the group should revisit
  once the real data arrives.
