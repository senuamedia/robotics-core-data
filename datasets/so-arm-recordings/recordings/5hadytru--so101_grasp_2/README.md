# 5hadytru/so101_grasp_2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [5hadytru/so101_grasp_2](https://huggingface.co/datasets/5hadytru/so101_grasp_2), revision `e6c19502df90f035327b714ea8310830d71d3adc` |
| Author | Truman Hickok ([5hadytru](https://huggingface.co/5hadytru)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 780 episodes, 522,332 frames |
| Rank | 21 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09814 of LeRobot's calibrated scale (a range of 2038 counts) | 1606 | 1794 |
| shoulder lift | 0.08522 of LeRobot's calibrated scale (a range of 2347 counts) | 1858 | 1873 |
| elbow flex | 0.09079 of LeRobot's calibrated scale (a range of 2203 counts) | 2184 | 2204 |
| wrist flex | 0.08772 of LeRobot's calibrated scale (a range of 2280 counts) | 1262 | 1292 |
| wrist roll | 0.05238 of LeRobot's calibrated scale (a range of 3818 counts) | 2463 | 2497 |
| gripper | 0.06849 of LeRobot's calibrated scale (a range of 1460 counts) | 827 | 708 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (283,743 frames); every frame then read as it would arrive. The held-out half (238,589 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 96 | 130 | 22 |
| shoulder lift | 85 | 134 | 40 |
| elbow flex | 52 | 148 | 33 |
| wrist flex | 80 | 138 | 35 |
| wrist roll | 45 | 182 | 36 |
| gripper | 23 | 198 | 64 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.427 | 1.531 | 3.229 | 2.205 |
| shoulder lift | 1.112 | 2.032 | 3.960 | 2.750 |
| elbow flex | 0.978 | 1.995 | 4.362 | 3.202 |
| wrist flex | 0.562 | 1.251 | 2.823 | 1.948 |
| wrist roll | 0.423 | 1.218 | 2.641 | 1.774 |
| gripper | 2.005 | 2.087 | 3.005 | 2.433 |
| five body joints | 0.757 | 1.642 | 3.466 | 2.433 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
