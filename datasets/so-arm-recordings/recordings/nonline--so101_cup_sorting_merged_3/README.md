# nonline/so101_cup_sorting_merged_3

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [nonline/so101_cup_sorting_merged_3](https://huggingface.co/datasets/nonline/so101_cup_sorting_merged_3), revision `676295bf8b8d5a5f1ef90022933ab84e1f50e4f6` |
| Author | N H ([nonline](https://huggingface.co/nonline)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 305 episodes, 230,588 frames |
| Rank | 62 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08768 of LeRobot's calibrated scale (a range of 2281 counts) | 1427 | 1425 |
| shoulder lift | 0.08507 of LeRobot's calibrated scale (a range of 2351 counts) | 2035 | 2051 |
| elbow flex | 0.09021 of LeRobot's calibrated scale (a range of 2217 counts) | 2187 | 2198 |
| wrist flex | 0.08703 of LeRobot's calibrated scale (a range of 2298 counts) | 932 | 949 |
| wrist roll | 0.06803 of LeRobot's calibrated scale (a range of 2940 counts) | 1769 | 2278 |
| gripper | 0.06680 of LeRobot's calibrated scale (a range of 1497 counts) | 570 | 501 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (217,830 frames); every frame then read as it would arrive. The held-out half (244,950 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 223 | 224 | 925 |
| shoulder lift | 68 | 270 | 1227 |
| elbow flex | 31 | 324 | 954 |
| wrist flex | 121 | 211 | 703 |
| wrist roll | 55 | 231 | 904 |
| gripper | 12 | 192 | 631 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 155.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.581 | 3.404 | 4.595 | 3.851 |
| shoulder lift | 2.503 | 6.394 | 7.702 | 6.556 |
| elbow flex | 1.322 | 5.180 | 7.345 | 6.339 |
| wrist flex | 0.428 | 1.042 | 1.521 | 1.281 |
| wrist roll | 0.607 | 3.070 | 4.049 | 3.431 |
| gripper | 1.538 | 3.793 | 4.943 | 4.311 |
| five body joints | 1.334 | 4.238 | 5.533 | 4.720 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
