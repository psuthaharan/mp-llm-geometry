# MP–LLM Geometry

Reproducibility materials for **“Inside self-reflection: the geometry in
multi-dimensional scoring of metacognitive structure.”** The analysis uses the
base `google/gemma-2-2b` model and de-identified data from 486 participants.

The reproduction entry point is
[`mp_llm_geometry_analysis.ipynb`](mp_llm_geometry_analysis.ipynb). It regenerates
the reported statistical summaries and all four manuscript figures.

## Reproduction scope

The default workflow is designed for reviewers: it validates and reuses the
shared activation checkpoint, full-vocabulary couplings, and pair-level
activation-patching caches. It then recomputes the statistical analyses,
uncertainty estimates, tables, and figures.

- Figures 1–2: recomputed from participant data and cached activations.
- Figure 3: corpus analyses are recomputed; the expensive full-vocabulary sweep
  is loaded from its validated cache by default.
- Figure 4: statistics and plots are recomputed from validated pair-level
  live-forward caches.

The repository does **not** claim a fully from-scratch reproduction of the
Figure 4 live-forward patching caches. Those model-forward results are shared
as auditable pair-level CSV files. Set `RECOMPUTE_ACTIVATIONS = True` only to
regenerate the activation checkpoint, and `RECOMPUTE_FULL_VOCAB = True` only to
rerun the full-vocabulary GPU sweep.

## Quick start

Python 3.11.9 was used for the verified run.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Gemma-2-2B is gated on Hugging Face. Accept its license on the model page and
authenticate before running Figure 3:

```powershell
hf auth login
```

Run the notebook from the repository root:

```powershell
python -m nbconvert --to notebook --execute --inplace `
  mp_llm_geometry_analysis.ipynb --ExecutePreprocessor.timeout=-1
```

A CUDA-capable GPU is recommended. The bundled activation and
full-vocabulary caches avoid the two most expensive forward/sweep operations,
but Figure 3 still requires access to the Gemma tokenizer and unembedding
weights. The 5,000-draw bootstrap analyses can take substantial time.

## Inputs and outputs

Required shared inputs:

- `data/prl_metacognition_deidentified.csv` — participant-level source table.
- `data/gemma2_2b_mean_pooled_residual_stream.npz` — L0–L25 mean-pooled
  residual-stream activations.
- `data/figure3_full_vocabulary_couplings.npz` — cached full-vocabulary
  coupling vectors.
- `data/empirical_pair_effects.csv` — 30 matched pairs × 26 patch layers.
- `data/specificity_l19_pair_effects.csv` — nine alternative MP contrasts.
- `data/random_direction_l19_pair_effects.csv` — 100 random directions at L19.

Generated outputs are written to `data/figure*.csv` and `figures/`. The main
manuscript uses `fig1.png`, `fig2.png`, `fig3.png`, and `fig4.png`; appendix
figures are also included.

`data/live_baselines.csv` is a quality-assurance artifact used to compare live
and cached baseline predictions. It is retained for provenance but is not
consumed by the analysis notebook.

## Expected manuscript checkpoints

A successful run should recover:

- hierarchy alignment: `r = 0.971`, exact permutation `p = 0.0167`;
- Evaluation encoding at L12: held-out `r = 0.579`;
- Evaluation as the lowest-coupling dimension in 91.7% of bootstrap draws;
- Figure 3 token-profile correlations: Evaluation `rho = 0.053`,
  Final Decision `rho = -0.675`;
- L19 patching effect: mean delta `-0.1168`, 95% CI `[-0.1951, -0.0426]`,
  max-statistic familywise `p = 0.0715`.

Machine-readable values are in the `data/figure*_reported_summary.csv` files.

## Integrity and data use

Verify shared artifacts against [`CHECKSUMS.sha256`](CHECKSUMS.sha256) before
running. See [`DATA.md`](DATA.md) for provenance, variable definitions,
human-participant safeguards, and the distinction between source, cache, and
derived files.

The analysis code is available under the [MIT License](LICENSE). That license
does not apply to participant data, manuscript text, Gemma weights, or other
third-party materials; their respective terms continue to apply.
