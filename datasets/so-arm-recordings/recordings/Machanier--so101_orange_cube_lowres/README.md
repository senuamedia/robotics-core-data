# Machanier/so101_orange_cube_lowres

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Machanier/so101_orange_cube_lowres](https://huggingface.co/datasets/Machanier/so101_orange_cube_lowres), revision `7bc5f0d7ec86789a3cb8b6efaa3f04f7ca341c24` |
| Author | Maxence Chanier ([Machanier](https://huggingface.co/Machanier)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 201 episodes, 144,751 frames |
| Rank | 119 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1782 | 1805 |
| shoulder lift | 360/4,095 degree (degrees) | 2317 | 2298 |
| elbow flex | 360/4,095 degree (degrees) | 2111 | 2183 |
| wrist flex | 360/4,095 degree (degrees) | 1299 | 1334 |
| wrist roll | 360/4,095 degree (degrees) | 1372 | 1399 |
| gripper | 0.06748 of LeRobot's calibrated scale (a range of 1482 counts) | 757 | 652 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (85,761 frames); every frame then read as it would arrive. The held-out half (58,990 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 64 | 75 | 57 |
| shoulder lift | 67 | 79 | 0 |
| elbow flex | 51 | 94 | 15 |
| wrist flex | 64 | 98 | 40 |
| wrist roll | 67 | 83 | 18 |
| gripper | 24 | 96 | 6 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.472 | 0.835 | 1.745 | 1.204 |
| shoulder lift | 1.267 | 1.741 | 3.289 | 2.816 |
| elbow flex | 1.330 | 2.261 | 4.184 | 3.581 |
| wrist flex | 2.978 | 3.018 | 1.815 | 1.313 |
| wrist roll | 0.479 | 0.509 | 1.338 | 0.924 |
| gripper | 3.932 | 4.132 | 5.226 | 4.637 |
| five body joints | 1.593 | 1.908 | 2.700 | 2.226 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
