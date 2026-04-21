# Tarpit

Workspace for systematically exploring the Nock 4K formula space with a Jupyter notebook and Parquet outputs.

The main artifact is `nock_exploration.ipynb`, which enumerates Nock formulas over a bounded atom range, evaluates them against a fixed subject corpus with the local `pinochle` interpreter, and writes the results to Parquet for later analysis.

## What is here

| Path | Purpose |
| --- | --- |
| `nock_exploration.ipynb` | End-to-end exploration notebook: setup, instrumentation, corpus generation, evaluation sweep, and analysis. |
| `*.parquet` | Generated datasets from full sweeps and targeted runs. |
| `pinochle/` | Local checkout of the `pinochle` project, which supplies the Nock interpreter and related tooling. |

## Exploration summary

The notebook explores full binary-tree Nock formulas by:

1. generating every formula shape for selected leaf counts,
2. filling leaves with atoms from a configurable range,
3. evaluating each formula against a small corpus of subjects, and
4. recording validity, crash reason, result shape, size, depth, and step count.

The default notebook configuration uses:

- atom values `0..11`,
- formula leaf counts `2..6`, and
- a subject corpus of 12 atoms plus 5 small cells.

Results are written incrementally to Parquet so analysis can be re-run without repeating the full sweep.

## Generated data files

Current outputs in the repo root include:

- `nock_s23.parquet`
- `nock_s4.parquet`
- `nock_a12_s2_3_4_5.parquet`
- `nock_a12_s2_3_4_5_6.parquet`
- `nock_s5_targeted.parquet`
- `nock_targeted_size7_a12.parquet`

These are notebook outputs, not hand-maintained source files.

## Running the notebook

The notebook expects a local `pinochle` checkout and imports the core package directly from:

```python
Path.home() / "urbit/tarpit/pinochle/packages/pinochle"
```

To reproduce the environment from this repo layout:

```bash
python -m venv .venv
source .venv/bin/activate
pip install jupyter pandas matplotlib tqdm pyarrow
pip install ./pinochle/packages/pinochle
jupyter notebook nock_exploration.ipynb
```

If you keep `pinochle` somewhere else, update the path in the notebook setup cell.

## Notes

- `pinochle/` is its own project with package and kernel documentation under its own `README.md` files.
- `.ipynb_checkpoints/` and additional Parquet files are generated working artifacts.
- This repository is primarily a research workspace for enumeration and analysis rather than a packaged application.
