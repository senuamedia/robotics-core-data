# HarrisonLee24/so100_test

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [HarrisonLee24/so100_test](https://huggingface.co/datasets/HarrisonLee24/so100_test), revision `f5f0286507931588271361c47de6cd0a5fb6b620` |
| Author | [HarrisonLee24](https://huggingface.co/HarrisonLee24) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 92 episodes, 80,091 frames |
| Rank | 267 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1198 | 1208 |
| shoulder lift | 360/4,096 degree (degrees) | 1558 | 1564 |
| elbow flex | 360/4,096 degree (degrees) | 1645 | 1718 |
| wrist flex | 360/4,096 degree (degrees) | 924 | 970 |
| wrist roll | 360/4,096 degree (degrees) | 1987 | 1986 |
| gripper | 0.08361 of LeRobot's calibrated scale (a range of 1196 counts) | 691 | 634 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (39,998 frames); every frame then read as it would arrive. The held-out half (40,093 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 58 | 140 | 9 |
| shoulder lift | 49 | 197 | 2 |
| elbow flex | 76 | 216 | 14 |
| wrist flex | 67 | 173 | 7 |
| wrist roll | 99 | 200 | 25 |
| gripper | 14 | 195 | 2 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
