# Metacognitive monitoring in translation: data and analysis code

This repository contains the data and analysis code for the study applying
Type-2 signal detection theory (AUROC2) to translation-process data, using
the CRITT TPR-DB corpora BML12 (English–Spanish) and SG12 (English–German).

The analysis measures whether translators' Drafting reformulation
behaviour discriminates initial productions that survive into the final
text ("correct") from those that do not ("incorrect") — an implicit,
behavioural index of metacognitive sensitivity.

## Contents

| File | Description |
|------|-------------|
| `data_pipeline.ipynb` | Builds the trial-level datasets from the raw TPR-DB tables: segments production units (PUs), classifies reformulations under the content filtering condition (Section 2.3), applies the Task 1 (survival) and Task 2 (reformulation) coding, and writes the two trial CSVs below. |
| `statistical_tests.ipynb` | All AUROC2 analyses: per-participant and group AUROC2 under the lower and upper bounds, contingency tables, threshold-sensitivity analysis, per-participant distributions, the accuracy–sensitivity dissociation, the variance decomposition, and the confound (t-sweep) and self-repair leak diagnostics. Reproduces every table and figure in the paper. |
| `BML12_2bin_trials.csv` | Trial-level data for the English–Spanish corpus (one row per gradeable production unit). |
| `SG12_2bin_trials.csv` | Trial-level data for the English–German corpus (one row per gradeable production unit). |

## Trial data columns

Each row in the two `*_2bin_trials.csv` files is one initial production unit.
Key columns:

- `Part`, `Session` — participant and session identifiers.
- `Task_1` — 1 if the production survived into the final text, 0 if not.
- `Task_1_Score` — fuzzy-match score (0–100) against the final text; `Task_1`
  is `Task_1_Score >= 75` under the primary criterion.
- `Task_2` — reformulation response under the **lower bound**: 1 = reformulated
  by the immediately following PU (low implicit confidence), 2 = not
  reformulated (high implicit confidence).
- `Task_2_Upper` — reformulation response under the **upper bound**: as `Task_2`,
  but counting reformulation by *any* later drafting PU in the same session.
- `Prev_PU_Edit`, `Next_PU_Edit` — CRITT edit strings for the preceding and
  following units (bracketed spans mark deleted characters).
- `Pause` — inter-unit pause in milliseconds.
- Remaining columns are intermediate fields used by the pipeline
  (phase tags, span indices, word-change measures, deletion flags).

## AUROC2 definition

AUROC2 = ½·[H₂ + (1 − F₂)], where the "signal" is a correct production and a
"high-confidence" response is *not* reformulating:

- H₂ = P(not reformulated | correct)  (hit rate)
- F₂ = P(not reformulated | incorrect)  (false-alarm rate)

Group and pooled values are means across participants, with 95% bootstrap
confidence intervals resampling participants (the translator is the unit of
inference). This is computed both via `sklearn.metrics.roc_auc_score` and via
the explicit formula above; the two agree to machine precision.

## Reproducing the results

1. Install dependencies: `pandas`, `numpy`, `scipy`, `scikit-learn`,
   `matplotlib`.
2. Run `data_pipeline.ipynb` to regenerate the trial CSVs from the raw
   TPR-DB tables (or use the provided CSVs directly).
3. Run `statistical_tests.ipynb` top to bottom to reproduce all tables and
   figures.

The trial CSVs are provided so that `statistical_tests.ipynb` can be run on
its own without the raw corpus.

## Source data

The raw process data derive from the CRITT Translation Process Research
Database (TPR-DB), studies BML12 and SG12. The TPR-DB is a publicly available
repository of user activity data (keystroke and gaze logs) from translation
studies, maintained by the Center for Research and Innovation in Translation
and Translation Technology (CRITT). See the TPR-DB for the original logs:
https://sites.google.com/site/centretranslationinnovation/tpr-db

## Citation

If you use this data or code, please cite the accompanying paper and
acknowledge the CRITT TPR-DB.

Aishvarya Raj (2026). Metacognitive Monitoring in Naturalistic Translation: a Signal Detection Approach

## License

**Data** (`BML12_2bin_trials.csv`, `SG12_2bin_trials.csv`,`data_pipeline.ipynb`, `statistical_tests.ipynb`) is derived from the
CRITT TPR-DB (studies BML12 and SG12), which is licensed under a Creative
Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0). As
a derivative work, this data is released under the same license,
**CC BY-NC 4.0**: it may be shared and adapted for non-commercial purposes
with attribution to both this work and the CRITT TPR-DB.

Original data: Center for Research and Innovation in Translation and
Translation Technology (CRITT) / Kent State
University.

```
Copyright (c) 2026 Aishvarya Raj

