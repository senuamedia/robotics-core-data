# liuhuanjim013/so100_th_1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [liuhuanjim013/so100_th_1](https://huggingface.co/datasets/liuhuanjim013/so100_th_1), revision `1f99c9370054bbc03b4f9338b2af314375046b2c` |
| Author | [liuhuanjim013](https://huggingface.co/liuhuanjim013) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 210 episodes, 76,123 frames |
| Rank | 281 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1717 | 1753 |
| shoulder lift | 360/4,096 degree (degrees) | 2109 | 2111 |
| elbow flex | 360/4,096 degree (degrees) | 2177 | 2203 |
| wrist flex | 360/4,096 degree (degrees) | 925 | 942 |
| wrist roll | 360/4,096 degree (degrees) | 2529 | 2609 |
| gripper | 0.08598 of LeRobot's calibrated scale (a range of 1163 counts) | 560 | 593 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,618 frames); every frame then read as it would arrive. The held-out half (31,505 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 26 | 137 | 46 |
| shoulder lift | 62 | 189 | 54 |
| elbow flex | 42 | 158 | 51 |
| wrist flex | 61 | 109 | 45 |
| wrist roll | 78 | 179 | 24 |
| gripper | 25 | 114 | 6 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
