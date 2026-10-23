# pranavsaroha/so100_medicine_0605

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [pranavsaroha/so100_medicine_0605](https://huggingface.co/datasets/pranavsaroha/so100_medicine_0605), revision `3838f8c932d933804a79d17f52c456b59932840b` |
| Author | [pranavsaroha](https://huggingface.co/pranavsaroha) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 40 episodes, 90,315 frames |
| Rank | 228 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1058 | 1149 |
| shoulder lift | 360/4,096 degree (degrees) | 1278 | 1254 |
| elbow flex | 360/4,096 degree (degrees) | 1498 | 1561 |
| wrist flex | 360/4,096 degree (degrees) | 1363 | 1377 |
| wrist roll | 360/4,096 degree (degrees) | 1939 | 1997 |
| gripper | 0.09718 of LeRobot's calibrated scale (a range of 1029 counts) | 428 | 447 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (48,585 frames); every frame then read as it would arrive. The held-out half (41,730 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 34 | 81 | 14 |
| shoulder lift | 27 | 123 | 16 |
| elbow flex | 38 | 127 | 3 |
| wrist flex | 25 | 119 | 16 |
| wrist roll | 37 | 141 | 36 |
| gripper | 23 | 111 | 25 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
