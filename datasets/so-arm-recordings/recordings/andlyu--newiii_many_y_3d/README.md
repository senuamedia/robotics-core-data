# andlyu/newiii_many_y_3d

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [andlyu/newiii_many_y_3d](https://huggingface.co/datasets/andlyu/newiii_many_y_3d), revision `9a067be8f80b9e5b30ec934b0b77703ba16e9b8f` |
| Author | Andrew Lyubovsky ([andlyu](https://huggingface.co/andlyu)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 75 episodes, 139,383 frames |
| Rank | 125 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1067 | 1150 |
| shoulder lift | 360/4,096 degree (degrees) | 1856 | 1848 |
| elbow flex | 360/4,096 degree (degrees) | 1215 | 1314 |
| wrist flex | 360/4,096 degree (degrees) | 1853 | 1880 |
| wrist roll | 360/4,096 degree (degrees) | 1577 | 1597 |
| gripper | 0.10040 of LeRobot's calibrated scale (a range of 996 counts) | 157 | 184 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (64,194 frames); every frame then read as it would arrive. The held-out half (80,465 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 41 | 78 | 5 |
| shoulder lift | 50 | 91 | 6 |
| elbow flex | 48 | 90 | 2 |
| wrist flex | 30 | 81 | 12 |
| wrist roll | 24 | 81 | 16 |
| gripper | 17 | 62 | 1 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
