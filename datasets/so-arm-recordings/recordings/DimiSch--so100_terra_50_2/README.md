# DimiSch/so100_terra_50_2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [DimiSch/so100_terra_50_2](https://huggingface.co/datasets/DimiSch/so100_terra_50_2), revision `6610dd65f72a0d6e997794893252fe021920bbe0` |
| Author | [DimiSch](https://huggingface.co/DimiSch) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.0, 30 frames a second |
| Recording | 34 episodes, 81,062 frames |
| Rank | 263 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 885 | 947 |
| shoulder lift | 360/4,096 degree (degrees) | 1731 | 1757 |
| elbow flex | 360/4,096 degree (degrees) | 1759 | 1825 |
| wrist flex | 360/4,096 degree (degrees) | 934 | 960 |
| wrist roll | 360/4,096 degree (degrees) | 1161 | 1188 |
| gripper | 0.06489 of LeRobot's calibrated scale (a range of 1541 counts) | 1079 | 820 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (40,529 frames); every frame then read as it would arrive. The held-out half (40,533 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 49 | 68 | 61 |
| shoulder lift | 47 | 109 | 18 |
| elbow flex | 50 | 139 | 19 |
| wrist flex | 53 | 87 | 27 |
| wrist roll | 62 | 73 | 27 |
| gripper | 26 | 165 | 5 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
