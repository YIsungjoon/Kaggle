# The 2026 NeuroGolf Championship

Kaggle competition: <https://www.kaggle.com/competitions/neurogolf-2026>

Goal: design the smallest possible ONNX neural networks that solve ARC-AGI image transformations.

## Local Paths

```text
data/neurogolf-2026/                  # raw task JSON files, ignored by git
competitions/neurogolf-2026/docs/     # stable competition notes
competitions/neurogolf-2026/notes/    # working notes and task analysis
competitions/neurogolf-2026/notebooks/# exploratory notebooks
competitions/neurogolf-2026/src/      # reusable code
competitions/neurogolf-2026/submissions/
```

## Reference Notes

- [Important constraints](docs/constraints.md)
- [Data notes](docs/data.md)

## Current Understanding

- Each task is defined by `train`, `test`, and `arc-gen` input/output grid pairs.
- Submitted solutions are task-specific ONNX networks.
- Correctness is exact-match over one-hot output tensors.
- Smaller parameter count and memory footprint produce better scores.
- Public examples should not be memorized; private validation checks generalization.

## Working Log

Add dated entries here as experiments begin.

```text
2026-05-25
- Joined the competition.
- Recorded overview constraints and dataset contract.
- Found that local arc-gen contains some examples larger than 30x30, despite the stated 30x30 tensor contract.
```
