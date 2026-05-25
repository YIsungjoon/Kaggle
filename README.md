# Kaggle Competition History

This repository records my Kaggle competition work: notes, experiments, code, submissions, and retrospectives.

Raw competition data is kept locally under `data/` and is intentionally ignored by git. Competition-specific code and notes live under `competitions/`.

## Competitions

| Participation Date | Competition |
| --- | --- |
| 2026-05-25 | [The 2026 NeuroGolf Championship](competitions/neurogolf-2026/README.md) |

## Repository Layout

```text
competitions/
  neurogolf-2026/
    README.md
    docs/
    notes/
    notebooks/
    src/
    submissions/
data/
  neurogolf-2026/
```

## Tracking Policy

- Track: source code, notebooks, analysis notes, experiment logs, small metadata, and submission summaries.
- Do not track: raw Kaggle datasets, large generated model files, ONNX files, zip submissions, credentials, or local environments.
- When a large artifact matters, record how it was produced and where it was submitted instead of committing the artifact itself.
