# roboticshack/team16-cleaning-plate

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [roboticshack/team16-cleaning-plate](https://huggingface.co/datasets/roboticshack/team16-cleaning-plate), revision `62ed0e9eeace8be0cb740e1452dad3a1a8f50ac7` |
| Author | [roboticshack](https://huggingface.co/roboticshack) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 100 episodes, 74,893 frames |
| Rank | 290 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 937 | 945 |
| shoulder lift | 360/4,096 degree (degrees) | 1804 | 1797 |
| elbow flex | 360/4,096 degree (degrees) | 1955 | 2024 |
| wrist flex | 360/4,096 degree (degrees) | 1292 | 1302 |
| wrist roll | 360/4,096 degree (degrees) | 515 | 521 |
| gripper | 0.07153 of LeRobot's calibrated scale (a range of 1398 counts) | 764 | 614 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (37,444 frames); every frame then read as it would arrive. The held-out half (37,449 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 52 | 91 | 2 |
| shoulder lift | 32 | 124 | 7 |
| elbow flex | 93 | 139 | 7 |
| wrist flex | 75 | 126 | 0 |
| wrist roll | 59 | 80 | 3 |
| gripper | 24 | 112 | 9 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
