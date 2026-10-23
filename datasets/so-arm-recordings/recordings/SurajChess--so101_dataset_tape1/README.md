# SurajChess/so101_dataset_tape1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [SurajChess/so101_dataset_tape1](https://huggingface.co/datasets/SurajChess/so101_dataset_tape1), revision `985b70506256800b641578291e7fe9314009861c` |
| Author | [SurajChess](https://huggingface.co/SurajChess) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 250 episodes, 76,216 frames |
| Rank | 280 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09179 of LeRobot's calibrated scale (a range of 2179 counts) | 1917 | 2461 |
| shoulder lift | 0.08389 of LeRobot's calibrated scale (a range of 2384 counts) | 1949 | 2023 |
| elbow flex | 0.08869 of LeRobot's calibrated scale (a range of 2255 counts) | 1848 | 1884 |
| wrist flex | 0.08522 of LeRobot's calibrated scale (a range of 2347 counts) | 1203 | 1255 |
| wrist roll | 0.05165 of LeRobot's calibrated scale (a range of 3872 counts) | 817 | 848 |
| gripper | 0.06627 of LeRobot's calibrated scale (a range of 1509 counts) | 392 | 372 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (42,533 frames); every frame then read as it would arrive. The held-out half (33,683 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 22 | 133 | 37 |
| shoulder lift | 37 | 150 | 28 |
| elbow flex | 51 | 148 | 60 |
| wrist flex | 38 | 123 | 7 |
| wrist roll | 24 | 105 | 15 |
| gripper | 19 | 99 | 17 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.783 | 2.255 | 4.551 | 3.062 |
| shoulder lift | 1.606 | 2.797 | 5.521 | 3.700 |
| elbow flex | 1.364 | 2.691 | 5.295 | 3.629 |
| wrist flex | 0.626 | 0.931 | 1.809 | 1.251 |
| wrist roll | 0.451 | 0.566 | 1.131 | 0.865 |
| gripper | 2.343 | 2.410 | 3.028 | 2.580 |
| five body joints | 1.062 | 2.065 | 4.093 | 2.776 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
