# corona314/data15_20261024_111140

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [corona314/data15_20261024_111140](https://huggingface.co/datasets/corona314/data15_20261024_111140), revision `36c4a3ade206bfc4a2ae6272b6f0e0a7caf2294f` |
| Author | [corona314](https://huggingface.co/corona314) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 110 episodes, 86,990 frames |
| Rank | 246 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1209 | 1253 |
| shoulder lift | 360/4,095 degree (degrees) | 1987 | 2001 |
| elbow flex | 360/4,095 degree (degrees) | 2185 | 2212 |
| wrist flex | 360/4,095 degree (degrees) | 2282 | 2313 |
| wrist roll | 360/4,095 degree (degrees) | 1185 | 1213 |
| gripper | 0.07008 of LeRobot's calibrated scale (a range of 1427 counts) | 550 | 477 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (43,457 frames); every frame then read as it would arrive. The held-out half (43,533 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 55 | 102 | 22 |
| shoulder lift | 36 | 135 | 27 |
| elbow flex | 15 | 184 | 17 |
| wrist flex | 21 | 227 | 3 |
| wrist roll | 29 | 111 | 1 |
| gripper | 30 | 122 | 24 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 1.063 | 1.329 | 2.126 | 1.681 |
| shoulder lift | 1.296 | 2.015 | 3.844 | 2.737 |
| elbow flex | 1.029 | 1.987 | 3.920 | 2.676 |
| wrist flex | 1.383 | 2.808 | 5.246 | 3.606 |
| wrist roll | 0.416 | 0.491 | 1.319 | 0.867 |
| gripper | 4.316 | 4.332 | 4.455 | 4.364 |
| five body joints | 1.091 | 1.892 | 3.575 | 2.499 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
