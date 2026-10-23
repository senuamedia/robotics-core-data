# Youngermaster/so100_medicamentos

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Youngermaster/so100_medicamentos](https://huggingface.co/datasets/Youngermaster/so100_medicamentos), revision `5c254e5bad9e2d3bd234e693912d9c62ee7db148` |
| Author | Juan Manuel Young Hoyos ([Youngermaster](https://huggingface.co/Youngermaster)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 125,517 frames |
| Rank | 141 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 943 | 972 |
| shoulder lift | 360/4,095 degree (degrees) | 1873 | 1916 |
| elbow flex | 360/4,095 degree (degrees) | 1908 | 1959 |
| wrist flex | 360/4,095 degree (degrees) | 1038 | 1486 |
| wrist roll | 360/4,095 degree (degrees) | 1859 | 1888 |
| gripper | 0.08326 of LeRobot's calibrated scale (a range of 1201 counts) | 607 | 552 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (61,934 frames); every frame then read as it would arrive. The held-out half (63,583 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 47 | 69 | 33 |
| shoulder lift | 71 | 95 | 16 |
| elbow flex | 60 | 93 | 58 |
| wrist flex | 16 | 87 | 16 |
| wrist roll | 48 | 126 | 0 |
| gripper | 21 | 103 | 2 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.345 | 0.409 | 1.084 | 0.703 |
| shoulder lift | 2.282 | 2.388 | 2.529 | 1.934 |
| elbow flex | 0.992 | 1.230 | 2.794 | 2.279 |
| wrist flex | 1.832 | 1.836 | 1.814 | 1.698 |
| wrist roll | 0.254 | 0.254 | 0.335 | 0.334 |
| gripper | 4.456 | 4.456 | 4.492 | 4.451 |
| five body joints | 1.394 | 1.471 | 1.937 | 1.576 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
