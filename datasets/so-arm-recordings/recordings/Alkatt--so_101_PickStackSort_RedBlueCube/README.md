# Alkatt/so_101_PickStackSort_RedBlueCube

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Alkatt/so_101_PickStackSort_RedBlueCube](https://huggingface.co/datasets/Alkatt/so_101_PickStackSort_RedBlueCube), revision `35b45bf0403b69767002e32223f9ff7ea0e209b8` |
| Author | [Alkatt](https://huggingface.co/Alkatt) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 170 episodes, 106,294 frames |
| Rank | 178 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1968 | 1974 |
| shoulder lift | 360/4,095 degree (degrees) | 1941 | 1952 |
| elbow flex | 360/4,095 degree (degrees) | 1757 | 1820 |
| wrist flex | 360/4,095 degree (degrees) | 547 | 567 |
| wrist roll | 360/4,095 degree (degrees) | 750 | 760 |
| gripper | 0.07022 of LeRobot's calibrated scale (a range of 1424 counts) | 587 | 525 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (42,483 frames); every frame then read as it would arrive. The held-out half (63,811 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 30 | 87 | 4 |
| shoulder lift | 58 | 87 | 0 |
| elbow flex | 54 | 85 | 19 |
| wrist flex | 41 | 58 | 8 |
| wrist roll | 21 | 61 | 40 |
| gripper | 19 | 124 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.338 | 0.435 | 0.831 | 0.664 |
| shoulder lift | 1.668 | 1.957 | 3.284 | 2.521 |
| elbow flex | 0.950 | 1.343 | 3.475 | 2.967 |
| wrist flex | 0.355 | 0.391 | 1.031 | 0.736 |
| wrist roll | 0.265 | 0.239 | 0.755 | 0.509 |
| gripper | 4.453 | 4.464 | 4.841 | 4.545 |
| five body joints | 0.893 | 1.098 | 2.243 | 1.811 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
