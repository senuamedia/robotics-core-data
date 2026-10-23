# apayan/so100_ball8

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [apayan/so100_ball8](https://huggingface.co/datasets/apayan/so100_ball8), revision `f318cc9490b327bd1382e29c528f7d7348aa4716` |
| Author | [apayan](https://huggingface.co/apayan) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 104 episodes, 110,416 frames |
| Rank | 173 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 2083 | 2100 |
| shoulder lift | 360/4,096 degree (degrees) | 1963 | 2021 |
| elbow flex | 360/4,096 degree (degrees) | 1692 | 1758 |
| wrist flex | 360/4,096 degree (degrees) | 890 | 911 |
| wrist roll | 360/4,096 degree (degrees) | 1187 | 1200 |
| gripper | 0.06940 of LeRobot's calibrated scale (a range of 1441 counts) | 599 | 468 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (57,140 frames); every frame then read as it would arrive. The held-out half (53,276 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 48 | 76 | 26 |
| shoulder lift | 31 | 78 | 20 |
| elbow flex | 45 | 76 | 9 |
| wrist flex | 27 | 78 | 61 |
| wrist roll | 15 | 132 | 25 |
| gripper | 8 | 163 | 61 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
