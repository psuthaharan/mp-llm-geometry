# Data and artifact documentation

## Provenance and approved use

`data/prl_metacognition_deidentified.csv` contains the de-identified
participant-level table used in the manuscript. It is a secondary-analysis
dataset from the online probabilistic reversal-learning study reported by
Suthaharan et al. (2026); the manuscript states that participants gave informed
consent and that the data were made publicly available by the original
authors. The repository maintainer has confirmed that public redistribution of
this table, including reflection text, scoring rationales, demographics, and
study IDs, is covered by the applicable approval.

The table nevertheless contains human-participant information and potentially
sensitive variables. Do not attempt to identify participants, link rows to
external records, or use these data for individual-level psychological
inference. The task markers include measures related to paranoia. Any reuse
must follow the original study terms, institutional requirements, and
applicable law.

The repository's MIT license applies to analysis code only. It does not itself
license or expand permitted uses of the participant data.

## Participant table

The CSV contains 486 rows and 27 columns. Major variable groups are:

- `id`: study-specific participant identifier.
- `gender`, `race`, `education_level`: self-reported demographics.
- `cognitive_reflection_1`–`cognitive_reflection_3`: cognitive-reflection
  responses.
- `paranoia_score`, `referential_group`, `persecution_group`, and the sabotage
  item: study measures and derived groupings.
- `best_deck_chosen_proportion`, `wsr_avg`: reversal-learning behavior;
  `wsr_avg` is the win-switch rate analyzed in the manuscript.
- `mu02_avg`, `mu03_avg`, `kappa_avg`, `omega2_avg`, `omega3_avg`: computational
  model parameters; `mu03_avg` is the initial volatility prior used as a task
  marker.
- `strategy`, `switching`, `combined_reflection`: verbatim free-text
  reflections. `combined_reflection` is the model input.
- `comprehension_continuous`, `judgment_continuous`,
  `evaluation_continuous`, `final_decision_continuous`,
  `confidence_continuous`: the five continuous metacognitive-prompting rubric
  scores.
- `gpt_rationale`: scoring rationale associated with the rubric assessment.

Missingness and all analysis transformations are handled explicitly in the
notebook. Reflection length is measured in characters and nuisance effects are
removed within the relevant cross-validation workflow.

## Shared computational artifacts

- `gemma2_2b_mean_pooled_residual_stream.npz`: participant-aligned,
  mean-pooled Gemma-2-2B residual-stream vectors for layers L0–L25, plus
  participant identifiers and reflection lengths.
- `figure3_full_vocabulary_couplings.npz`: cached Evaluation-L12 and Final
  Decision-L21 coupling vectors over the Gemma vocabulary.
- `empirical_pair_effects.csv`: pair-level live-forward activation-patching
  effects for 30 directed matches over 26 layers.
- `specificity_l19_pair_effects.csv`: L19 effects for alternative
  metacognitive-dimension contrasts.
- `random_direction_l19_pair_effects.csv`: L19 effects for 100 isotropic random
  directions over 15 pairs.
- `live_baselines.csv`: retained quality-assurance comparison of live and
  cached baseline predictions; not consumed by the notebook.

These files are model-derived artifacts, not additional participants. Their
schemas, row counts, and alignment are checked by the notebook before
statistical aggregation.

## Derived outputs

Files beginning with `figure1_` through `figure4_` are generated summaries,
bootstrap draws, validation tables, or figure source data. They can be
overwritten by a clean notebook run. Files in `figures/` are likewise generated
outputs.

## Integrity

SHA-256 values for the participant table and shared caches are recorded in
`CHECKSUMS.sha256`. Verify them after download, especially when Git LFS is used
for `.npz` files.
