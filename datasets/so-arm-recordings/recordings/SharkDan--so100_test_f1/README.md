# SharkDan/so100_test_f1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [SharkDan/so100_test_f1](https://huggingface.co/datasets/SharkDan/so100_test_f1), revision `717a4c3b3e90f49c927f7efa57b06e5926a5edd7` |
| Author | Laiyidan ([SharkDan](https://huggingface.co/SharkDan)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 200 episodes, 120,674 frames |
| Rank | 145 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1113 | 1131 |
| shoulder lift | 360/4,096 degree (degrees) | 1586 | 1579 |
| elbow flex | 360/4,096 degree (degrees) | 1478 | 1511 |
| wrist flex | 360/4,096 degree (degrees) | 777 | 814 |
| wrist roll | 360/4,096 degree (degrees) | 1460 | 1510 |
| gripper | 0.08071 of LeRobot's calibrated scale (a range of 1239 counts) | 461 | 473 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (59,968 frames); every frame then read as it would arrive. The held-out half (61,275 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 130 | 27 |
| shoulder lift | 59 | 202 | 19 |
| elbow flex | 48 | 186 | 21 |
| wrist flex | 26 | 120 | 37 |
| wrist roll | 50 | 132 | 36 |
| gripper | 39 | 79 | 32 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
