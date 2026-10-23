# T1g3rGE/so100_pickplace_pnp

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [T1g3rGE/so100_pickplace_pnp](https://huggingface.co/datasets/T1g3rGE/so100_pickplace_pnp), revision `03261f0498edd3a4011974ea6379a7c12db53d6b` |
| Author | [T1g3rGE](https://huggingface.co/T1g3rGE) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 28 episodes, 75,282 frames |
| Rank | 284 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1472 | 1469 |
| shoulder lift | 360/4,096 degree (degrees) | 1757 | 1804 |
| elbow flex | 360/4,096 degree (degrees) | 1909 | 1995 |
| wrist flex | 360/4,096 degree (degrees) | 1166 | 1536 |
| wrist roll | 360/4,096 degree (degrees) | 1891 | 2460 |
| gripper | 0.09009 of LeRobot's calibrated scale (a range of 1110 counts) | 1196 | 1141 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (33,459 frames); every frame then read as it would arrive. The held-out half (41,823 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 56 | 112 | 17 |
| shoulder lift | 46 | 142 | 17 |
| elbow flex | 14 | 126 | 39 |
| wrist flex | 33 | 118 | 2 |
| wrist roll | 98 | 104 | 76 |
| gripper | 42 | 210 | 7 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
