# DorayakiLin/yzw_so100_stack_cube_v4

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [DorayakiLin/yzw_so100_stack_cube_v4](https://huggingface.co/datasets/DorayakiLin/yzw_so100_stack_cube_v4), revision `da7c2ecf66ab30bff1ee455a83efc83b37a7781c` |
| Author | [DorayakiLin](https://huggingface.co/DorayakiLin) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 200 episodes, 95,833 frames |
| Rank | 208 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 372 | 379 |
| shoulder lift | 360/4,096 degree (degrees) | 1715 | 1712 |
| elbow flex | 360/4,096 degree (degrees) | 1751 | 1786 |
| wrist flex | 360/4,096 degree (degrees) | 720 | 756 |
| wrist roll | 360/4,096 degree (degrees) | 437 | 448 |
| gripper | 0.08865 of LeRobot's calibrated scale (a range of 1128 counts) | 771 | 611 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (48,815 frames); every frame then read as it would arrive. The held-out half (47,018 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 34 | 34 | 1 |
| shoulder lift | 44 | 86 | 37 |
| elbow flex | 39 | 96 | 9 |
| wrist flex | 35 | 59 | 1 |
| wrist roll | 33 | 38 | 1 |
| gripper | 33 | 101 | 12 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
