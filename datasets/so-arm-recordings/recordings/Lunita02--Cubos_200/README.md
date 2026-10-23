# Lunita02/Cubos_200

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Lunita02/Cubos_200](https://huggingface.co/datasets/Lunita02/Cubos_200), revision `cbabbf1a05215f2b0424acb7eecfa5749ffef3cf` |
| Author | Selena Arribas Calderón ([Lunita02](https://huggingface.co/Lunita02)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 169,765 frames |
| Rank | 99 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1399 | 1426 |
| shoulder lift | 360/4,095 degree (degrees) | 1910 | 1924 |
| elbow flex | 360/4,095 degree (degrees) | 2077 | 2125 |
| wrist flex | 360/4,095 degree (degrees) | 1177 | 1204 |
| wrist roll | 360/4,095 degree (degrees) | 1623 | 1648 |
| gripper | 0.06382 of LeRobot's calibrated scale (a range of 1567 counts) | 640 | 557 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (68,211 frames); every frame then read as it would arrive. The held-out half (101,554 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 66 | 83 | 10 |
| shoulder lift | 38 | 134 | 0 |
| elbow flex | 55 | 148 | 0 |
| wrist flex | 58 | 109 | 12 |
| wrist roll | 53 | 139 | 0 |
| gripper | 15 | 117 | 3 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.395 | 0.469 | 0.897 | 0.721 |
| shoulder lift | 1.103 | 1.234 | 2.411 | 1.708 |
| elbow flex | 1.092 | 1.584 | 3.623 | 2.900 |
| wrist flex | 0.557 | 0.910 | 2.209 | 1.496 |
| wrist roll | 0.319 | 0.257 | 0.595 | 0.439 |
| gripper | 1.344 | 1.376 | 1.981 | 1.530 |
| five body joints | 0.771 | 1.014 | 2.235 | 1.690 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
