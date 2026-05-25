# NeuroGolf 2026 Important Constraints

Snapshot date: 2026-05-25

This document summarizes the competition constraints we should keep in view while building submissions for Kaggle's "The 2026 NeuroGolf Championship".

## Competition Goal

- Build the smallest possible neural networks that solve ARC-AGI image transformation tasks.
- Each task solution is submitted as an ONNX network.
- Correctness is required first; among correct networks, smaller networks score better.
- The contest is closer to rule discovery + ONNX circuit design + model golfing than ordinary model training.

## Submission Format

- Submit a single file named `submission.zip`.
- The zip may contain at most one ONNX file per task.
- Expected task file names:

```text
task001.onnx
task002.onnx
...
task400.onnx
```

- We can submit solutions for only the tasks we have solved.
- Each ONNX file must be at most `1.44MB`.

## ONNX Constraints

- All tensors must have statically defined shapes.
- All parameters must have statically defined shapes.
- The official network validator checks these constraints automatically.

Disallowed ONNX operations:

```text
Loop
Scan
NonZero
Unique
Script
Function
```

## Scoring

For each functionally correct task network:

```text
score = max(1, 25 - ln(cost))
cost = total_parameter_count + total_memory_footprint_in_bytes
```

Implications:

- Incorrect networks receive no useful score for that task.
- Smaller parameter count improves score.
- Smaller memory footprint improves score.
- Optimization should happen after correctness is established.

## Correctness Checks

Networks are validated against:

- Original ARC-AGI public training v1 benchmark tasks.
- ARC-GEN-100K.
- A small private benchmark suite.

Important implication:

- Avoid solutions that merely memorize public examples.
- Prefer networks that encode the underlying transformation rule.
- Public examples are not enough; solutions must generalize to related generated/private cases.

## Practical Development Checklist

Before submitting a task network:

- Confirm the ONNX file passes the official validator.
- Confirm the file size is below `1.44MB`.
- Confirm no disallowed ONNX ops are present.
- Confirm all tensor and parameter shapes are static.
- Confirm the network solves all visible examples for the task.
- Where possible, test on generated variations to reduce overfitting risk.
- Estimate or inspect cost: parameters + memory footprint in bytes.

## Strategy Notes

- Start with easy, high-confidence tasks.
- Group tasks by transformation type:
  - color replacement
  - object movement
  - symmetry
  - cropping or bounding boxes
  - pattern completion
  - copying or tiling
  - flood fill or region logic
- First build correct ONNX networks.
- Then reduce parameters, constants, intermediate tensors, and file size.
- Keep task-specific notes separate from this constraint document.

## Timeline

- Start date: 2026-04-15
- Entry deadline: 2026-07-08 23:59 UTC
- Team merger deadline: 2026-07-08 23:59 UTC
- Final submission deadline: 2026-07-15 23:59 UTC

Organizer announcements may change the metric or ban additional ONNX operators. Re-check the Kaggle Overview, Rules, and Discussions before major submissions.
