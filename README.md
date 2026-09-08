# Group 7 - Public Policy and Resource Allocation

CC2017 Modelacion y Simulacion - Ciudad UVG earthquake scenario.

Group 7 represents the technical unit of the Emergency Committee responsible
for allocating public resources (budget, heavy vehicles, distribution
vehicles, generators, fuel, water kits, tents) across the 5 urban zones of
Ciudad UVG (Z1..Z5) over the 72-hour post-event horizon (12 blocks of 6
hours each), and for evaluating that allocation against four policy metrics.

## Project layout

```
.
├── pyproject.toml           uv-managed dependency manifest (non-packaged project)
├── uv.lock                  locked dependency versions (commit this)
├── notebooks/
│   └── main.ipynb           THE deliverable: the entire model lives here
├── docs/
│   ├── Grupo7_PoliticaPublica.xlsx   the group's data file (source of truth)
│   ├── reporte_estructura.md         report skeleton (Spanish)
│   ├── video_guion.md                video outline (Spanish)
│   └── prompts_ia.md                 generative-AI prompt log (Spanish)
├── data/
│   ├── raw/                  drop any additional/updated Excel files here
│   └── exchange/              CSV reports received from Groups 2, 3, 5
└── outputs/                   exported figures and result tables
```

There is no `src/` package and no helper scripts: the assignment's
deliverable is the notebook itself, so every function, dataclass, and cell
lives directly in `notebooks/main.ipynb`, in the order the report/video
narrative needs them.

## Setup

This project uses [`uv`](https://docs.astral.sh/uv/) for dependency
management.

```bash
uv sync
uv run jupyter lab
```

Then open `notebooks/main.ipynb` and run all cells top to bottom.

To re-run and re-execute the notebook headlessly (e.g. to regenerate
`outputs/` after a change), instead run:

```bash
uv run jupyter nbconvert --to notebook --execute --inplace notebooks/main.ipynb
```

## Data status

The Group 7 Excel file (`docs/Grupo7_PoliticaPublica.xlsx`) already arrived
and its resource pools, zone damage table, and policy constraints are
hardcoded into the notebook's `Params` and constraint-checking cells. The
notebook also includes a parsing cell that re-reads the workbook directly
and cross-checks the hardcoded constants against it, so the notebook still
proves it reads the file correctly and still runs end to end if the file is
ever moved or unavailable.

The three **exchange** reports (from Groups 2, 3, and 5) do not exist yet -
they are only produced during the in-person exchange. Until then, the
notebook's exchange section falls back to a small synthetic placeholder
exchange so the "before/after" pipeline (Question 2) can run today.

### When the exchange data arrives

1. Drop the three CSV files (matching the schemas in
   `data/exchange/README.md`) into `data/exchange/`:
   `g2_hospital.csv`, `g3_supplies.csv`, `g5_personnel.csv`.
2. `uv sync` (only needed if dependencies changed).
3. `uv run jupyter lab` and re-run the notebook's exchange-loading cell -
   it automatically prefers the real files over the synthetic placeholder
   once they are present.
4. Re-run the Question 2 (before/after) and sensitivity-analysis cells.
5. Update the "Incorporacion del intercambio" section of
   `docs/reporte_estructura.md` with the real before/after findings.

### If a newer/updated Excel file arrives instead

1. Drop it in `data/raw/`.
2. `uv sync`.
3. `uv run jupyter lab` and run the Excel-parsing cell against the new path.
4. Update the hardcoded `Params` defaults and the constraint constants in
   the notebook from the newly printed schema.
5. Re-run all cells.

## Before the exchange / after the exchange workflow

- **Before the exchange:** the notebook builds `Params` purely from the
  Group 7 Excel file (resource pools, zone damage/vulnerability, declared
  priorities) and produces an initial 24-hour allocation proposal (Question
  1), evaluated against the four metrics with no external inputs.
- **At the exchange:** Group 7 should hand the CSV templates described in
  `data/exchange/README.md` to Groups 2, 3, and 5 so their reports come back
  in the exact shape the notebook expects.
- **After the exchange:** the notebook reloads with the `Exchange` bundle
  (hospital saturation from Group 2, supply bottlenecks from Group 3,
  personnel/reinforcement needs from Group 5), re-runs the Monte Carlo
  simulation, and produces the before/after comparison table with deltas and
  95% confidence intervals (Question 2), plus a sensitivity analysis on the
  national budget and on Z1 accessibility.

## Required final output (exam deliverable)

Per section 6 of `docs/Grupo7_PoliticaPublica.xlsx`, Group 7 delivers
directly to the professor (not to another group). The notebook's final
section produces all four parts and exports them to `outputs/`:

- **(a)** a 72-hour resource allocation plan per zone (`a_allocation_plan_72h.csv`)
- **(b)** a quantitative audit trail justifying every allocation decision (`b_audit_trail.csv`)
- **(c)** an evaluation of the plan against the four metrics, with pass/fail and 95% CI (`c_evaluation_vs_thresholds.csv`)
- **(d)** a sensitivity analysis on a halved national budget and on Z1 being inaccessible for the first 12 hours (`d_sensitivity_*.csv`)

## Paradigm

System Dynamics (aggregate stock-and-flow of resources, unmet need, and
backlog per zone) combined with a scheduled decision/control layer
(the allocation policy is re-evaluated every 6-hour block). This is
explicitly **not** an agent-based model (no individual decision-maker is the
object of study) and **not** a discrete-event simulation (no discrete-entity
queueing discipline). See the paradigm-justification markdown cell in the
notebook for the full argument.
