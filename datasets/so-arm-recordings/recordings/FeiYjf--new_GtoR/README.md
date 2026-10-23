# FeiYjf/new_GtoR

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [FeiYjf/new_GtoR](https://huggingface.co/datasets/FeiYjf/new_GtoR), revision `c3fba3ed78de36d602885c2d9569cff82fcff494` |
| Author | Fei ([FeiYjf](https://huggingface.co/FeiYjf)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.0, 30 frames a second |
| Recording | 378 episodes, 126,369 frames |
| Rank | 138 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1334 | 1347 |
| shoulder lift | 360/4,096 degree (degrees) | 1135 | 1191 |
| elbow flex | 360/4,096 degree (degrees) | 1451 | 1482 |
| wrist flex | 360/4,096 degree (degrees) | 688 | 718 |
| wrist roll | 360/4,096 degree (degrees) | 2232 | 2264 |
| gripper | 0.10215 of LeRobot's calibrated scale (a range of 979 counts) | 21 | 43 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (69,023 frames); every frame then read as it would arrive. The held-out half (57,346 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 105 | 127 | 4 |
| shoulder lift | 51 | 122 | 36 |
| elbow flex | 48 | 145 | 26 |
| wrist flex | 21 | 106 | 1 |
| wrist roll | 34 | 207 | 1 |
| gripper | 9 | 11 | 0 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
