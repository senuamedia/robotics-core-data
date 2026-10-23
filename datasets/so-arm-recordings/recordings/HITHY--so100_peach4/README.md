# HITHY/so100_peach4

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [HITHY/so100_peach4](https://huggingface.co/datasets/HITHY/so100_peach4), revision `e63a36d0e6be83c48d0223a913aef4d8da4f0851` |
| Author | [HITHY](https://huggingface.co/HITHY) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.0, 30 frames a second |
| Recording | 400 episodes, 98,409 frames |
| Rank | 205 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1062 | 1096 |
| shoulder lift | 360/4,096 degree (degrees) | 2125 | 2142 |
| elbow flex | 360/4,096 degree (degrees) | 2095 | 2126 |
| wrist flex | 360/4,096 degree (degrees) | 1857 | 1892 |
| wrist roll | 360/4,096 degree (degrees) | 1864 | 1853 |
| gripper | 0.07987 of LeRobot's calibrated scale (a range of 1252 counts) | 1170 | 1073 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (58,395 frames); every frame then read as it would arrive. The held-out half (40,014 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 48 | 71 | 73 |
| shoulder lift | 25 | 153 | 223 |
| elbow flex | 43 | 171 | 450 |
| wrist flex | 22 | 162 | 60 |
| wrist roll | 54 | 171 | 496 |
| gripper | 23 | 218 | 22 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
