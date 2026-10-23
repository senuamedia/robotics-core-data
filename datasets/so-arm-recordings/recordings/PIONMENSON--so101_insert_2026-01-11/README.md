# PIONMENSON/so101_insert_2026-01-11

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [PIONMENSON/so101_insert_2026-01-11](https://huggingface.co/datasets/PIONMENSON/so101_insert_2026-01-11), revision `72e8c14cd8129ef4e0bb18d7ef2c8db06fe83e69` |
| Author | [PIONMENSON](https://huggingface.co/PIONMENSON) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 79,387 frames |
| Rank | 270 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08803 of LeRobot's calibrated scale (a range of 2272 counts) | 659 | 680 |
| shoulder lift | 0.08457 of LeRobot's calibrated scale (a range of 2365 counts) | 1586 | 1605 |
| elbow flex | 0.09042 of LeRobot's calibrated scale (a range of 2212 counts) | 1556 | 1605 |
| wrist flex | 0.08610 of LeRobot's calibrated scale (a range of 2323 counts) | 847 | 892 |
| wrist roll | 0.05278 of LeRobot's calibrated scale (a range of 3789 counts) | 862 | 885 |
| gripper | 0.06964 of LeRobot's calibrated scale (a range of 1436 counts) | 607 | 600 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (41,145 frames); every frame then read as it would arrive. The held-out half (38,242 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 33 | 52 | 6 |
| shoulder lift | 29 | 101 | 9 |
| elbow flex | 22 | 98 | 0 |
| wrist flex | 36 | 96 | 0 |
| wrist roll | 51 | 67 | 29 |
| gripper | 28 | 86 | 19 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.323 | 0.461 | 1.202 | 0.848 |
| shoulder lift | 2.148 | 2.641 | 3.507 | 2.754 |
| elbow flex | 1.477 | 1.757 | 3.643 | 3.186 |
| wrist flex | 1.608 | 1.731 | 1.830 | 1.352 |
| wrist roll | 0.352 | 0.397 | 1.235 | 0.790 |
| gripper | 3.858 | 3.943 | 4.276 | 4.035 |
| five body joints | 1.386 | 1.638 | 2.525 | 2.045 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
