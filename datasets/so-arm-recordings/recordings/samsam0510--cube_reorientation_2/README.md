# samsam0510/cube_reorientation_2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [samsam0510/cube_reorientation_2](https://huggingface.co/datasets/samsam0510/cube_reorientation_2), revision `01a6af3fba3da975571f751d33d3b04b3431ac6a` |
| Author | Minjun Lee ([samsam0510](https://huggingface.co/samsam0510)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.0, 30 frames a second |
| Recording | 182 episodes, 115,586 frames |
| Rank | 161 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1012 | 1042 |
| shoulder lift | 360/4,096 degree (degrees) | 2171 | 2132 |
| elbow flex | 360/4,096 degree (degrees) | 2189 | 2196 |
| wrist flex | 360/4,096 degree (degrees) | 1227 | 1266 |
| wrist roll | 360/4,096 degree (degrees) | 726 | 1225 |
| gripper | 0.07047 of LeRobot's calibrated scale (a range of 1419 counts) | 197 | 186 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (62,394 frames); every frame then read as it would arrive. The held-out half (53,192 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 79 | 6 |
| shoulder lift | 54 | 172 | 24 |
| elbow flex | 70 | 189 | 12 |
| wrist flex | 66 | 111 | 16 |
| wrist roll | 43 | 82 | 2 |
| gripper | 34 | 35 | 33 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
