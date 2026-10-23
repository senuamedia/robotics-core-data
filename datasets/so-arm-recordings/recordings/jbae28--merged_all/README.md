# jbae28/merged_all

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [jbae28/merged_all](https://huggingface.co/datasets/jbae28/merged_all), revision `61cd2cd535881946dcc0e218b0cfb3cea3d72f2c` |
| Author | Jackson Bae ([jbae28](https://huggingface.co/jbae28)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 201 episodes, 120,392 frames |
| Rank | 146 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1135 | 1150 |
| shoulder lift | 360/4,095 degree (degrees) | 1780 | 1768 |
| elbow flex | 360/4,095 degree (degrees) | 1391 | 1439 |
| wrist flex | 360/4,095 degree (degrees) | 864 | 869 |
| wrist roll | 360/4,095 degree (degrees) | 1127 | 1138 |
| gripper | 0.06711 of LeRobot's calibrated scale (a range of 1490 counts) | 787 | 693 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (59,935 frames); every frame then read as it would arrive. The held-out half (60,457 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 43 | 139 | 37 |
| shoulder lift | 44 | 166 | 32 |
| elbow flex | 42 | 156 | 5 |
| wrist flex | 42 | 138 | 9 |
| wrist roll | 31 | 103 | 59 |
| gripper | 14 | 186 | 30 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.600 | 0.693 | 1.325 | 0.980 |
| shoulder lift | 1.398 | 1.826 | 2.571 | 2.025 |
| elbow flex | 0.887 | 1.076 | 2.605 | 2.442 |
| wrist flex | 3.481 | 3.498 | 1.523 | 1.247 |
| wrist roll | 0.269 | 0.150 | 0.531 | 0.396 |
| gripper | 11.159 | 11.191 | 11.198 | 11.100 |
| five body joints | 1.749 | 1.856 | 1.884 | 1.596 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
