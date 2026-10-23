# aHiroakiIshikawa/so101-fold-towel-merged

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [aHiroakiIshikawa/so101-fold-towel-merged](https://huggingface.co/datasets/aHiroakiIshikawa/so101-fold-towel-merged), revision `978352cb0f2e78a3dafdf114e9c462d74331e328` |
| Author | ishi ([aHiroakiIshikawa](https://huggingface.co/aHiroakiIshikawa)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 170 episodes, 154,903 frames |
| Rank | 107 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1122 | 1157 |
| shoulder lift | 360/4,095 degree (degrees) | 1833 | 1813 |
| elbow flex | 360/4,095 degree (degrees) | 1790 | 1863 |
| wrist flex | 360/4,095 degree (degrees) | 973 | 998 |
| wrist roll | 360/4,095 degree (degrees) | 1730 | 1739 |
| gripper | 0.06849 of LeRobot's calibrated scale (a range of 1460 counts) | 439 | 400 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (77,982 frames); every frame then read as it would arrive. The held-out half (76,921 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 55 | 5 |
| shoulder lift | 51 | 63 | 28 |
| elbow flex | 64 | 70 | 30 |
| wrist flex | 47 | 55 | 42 |
| wrist roll | 64 | 95 | 4 |
| gripper | 29 | 79 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 77.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.358 | 0.600 | 1.125 | 0.933 |
| shoulder lift | 1.262 | 2.021 | 3.443 | 2.957 |
| elbow flex | 1.213 | 2.438 | 4.497 | 3.891 |
| wrist flex | 2.170 | 2.235 | 1.509 | 1.152 |
| wrist roll | 0.442 | 0.771 | 1.561 | 1.132 |
| gripper | 1.604 | 1.626 | 1.988 | 1.814 |
| five body joints | 1.272 | 1.787 | 2.758 | 2.339 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
