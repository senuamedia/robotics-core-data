# fljjoin/so100_testcubo20

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [fljjoin/so100_testcubo20](https://huggingface.co/datasets/fljjoin/so100_testcubo20), revision `72610c55a02b2fb203427051338aac355ebe8ff8` |
| Author | Fablab Joinville ([fljjoin](https://huggingface.co/fljjoin)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.0, 30 frames a second |
| Recording | 40 episodes, 118,828 frames |
| Rank | 152 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1210 | 1244 |
| shoulder lift | 360/4,096 degree (degrees) | 1996 | 1970 |
| elbow flex | 360/4,096 degree (degrees) | 2197 | 2228 |
| wrist flex | 360/4,096 degree (degrees) | 840 | 868 |
| wrist roll | 360/4,096 degree (degrees) | 1667 | 1692 |
| gripper | 0.06925 of LeRobot's calibrated scale (a range of 1444 counts) | 1049 | 829 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (59,399 frames); every frame then read as it would arrive. The held-out half (59,429 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 60 | 105 | 20 |
| shoulder lift | 41 | 188 | 31 |
| elbow flex | 56 | 189 | 44 |
| wrist flex | 40 | 130 | 9 |
| wrist roll | 70 | 129 | 8 |
| gripper | 24 | 176 | 38 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
