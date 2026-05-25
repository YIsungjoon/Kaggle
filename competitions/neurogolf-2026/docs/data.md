# NeuroGolf 2026 Data Notes

Snapshot date: 2026-05-25

This document summarizes the dataset contract for Kaggle's "The 2026 NeuroGolf Championship" and records observations from the local dataset in `data/neurogolf-2026`.

## Files

- There are 400 task files.
- File names follow the submission task naming scheme:

```text
task001.json
task002.json
...
task400.json
```

- Each task file is a JSON dictionary with three fields:
  - `train`
  - `test`
  - `arc-gen`

## Pair Format

Each split contains a list of input/output pairs.

Each pair has this shape:

```json
{
  "input": [[0, 1, 2]],
  "output": [[0, 1, 2]]
}
```

- `input` is the source grid.
- `output` is the expected transformed grid.
- A grid is a rectangular list of lists.
- Cell values are integers from `0` through `9`, inclusive.

## Tensor Encoding

Before a grid is passed into a submitted network, it is encoded as:

```text
[BATCH_DIM=1, CHANNELS=10, HEIGHT=30, WIDTH=30]
```

Encoding rules:

- Color `0` maps to channel `0`.
- Color `1` maps to channel `1`.
- ...
- Color `9` maps to channel `9`.
- A real grid cell is one-hot encoded.
- A cell outside the original grid border is zero-hot encoded: all 10 channels are `0`.

Important distinction:

- Color `0` inside the grid is not the same as a clear pixel outside the border.
- Inside-grid color `0`: channel `0` is `1`.
- Outside-grid clear pixel: all channels are `0`.

## Output Contract

The submitted network must construct the expected output grid exactly.

For each output cell:

- The correct color channel should be `1`.
- All other color channels should be `0`.
- Cells outside the output grid border should be zero-hot across all channels.

Only exact matches count as correct.

## Validation Splits

Visible task files include:

- `train`: original ARC-AGI-1 training examples.
- `test`: original ARC-AGI-1 test examples.
- `arc-gen`: additional examples from ARC-GEN-100K.

The official scorer also uses a smaller private dataset per task to discourage overfitting.

## Local Dataset Summary

Observed in `data/neurogolf-2026`:

```text
task files: 400
total visible pairs: 101718

train pairs:   1302
test pairs:     416
arc-gen pairs: 100000
```

Per-task pair count ranges:

```text
train:   2 to 10 pairs per task
test:    1 to 3 pairs per task
arc-gen: 4 to 262 pairs per task
```

Visible grid size ranges:

```text
train input:   height 1-30, width 2-30
train output:  height 1-30, width 1-30

test input:    height 1-30, width 2-30
test output:   height 1-30, width 1-30

arc-gen input:  height 1-70, width 1-75
arc-gen output: height 1-42, width 1-38
```

## Size Caveat

The Kaggle dataset description says grids range from `1x1` to `30x30`, and the network input tensor is fixed at `1x10x30x30`.

However, the local `arc-gen` data includes some examples larger than `30x30`.

Observed examples above `30x30` appear only in `arc-gen`:

```text
total arc-gen pairs above 30x30 in at least one dimension: 371

task021: 189 / 262 arc-gen pairs
task184:  97 / 262 arc-gen pairs
task202:  37 / 262 arc-gen pairs
task080:  35 / 262 arc-gen pairs
task366:  11 / 262 arc-gen pairs
task055:   2 / 262 arc-gen pairs
```

Development implication:

- Treat `train` and `test` as matching the stated `30x30` tensor contract.
- Be careful when using all `arc-gen` examples for local validation.
- Any preprocessing or validator wrapper should explicitly decide how to handle `arc-gen` examples whose input or output shape exceeds `30x30`.
- Re-check Kaggle discussions or official tools before assuming how oversized `arc-gen` examples are handled by the scorer.

## Practical Development Notes

- Use `train`, `test`, and compatible `arc-gen` examples to infer the transformation rule.
- Avoid fitting only the visible examples; the private validation set may include related variations.
- For each task, record:
  - input/output shape pattern
  - colors used
  - transformation hypothesis
  - visible examples passed
  - `arc-gen` examples passed
  - whether oversized `arc-gen` examples were skipped or specially handled
