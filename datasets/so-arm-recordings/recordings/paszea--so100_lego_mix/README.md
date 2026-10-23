# paszea/so100_lego_mix

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [paszea/so100_lego_mix](https://huggingface.co/datasets/paszea/so100_lego_mix), revision `a2ee7ae91ce6776f75bf21b145dbd99b6e033f3a` |
| Author | [paszea](https://huggingface.co/paszea) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 50 episodes, 83,506 frames |
| Rank | 257 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 601 | 600 |
| shoulder lift | 360/4,096 degree (degrees) | 1712 | 1714 |
| elbow flex | 360/4,096 degree (degrees) | 1546 | 1645 |
| wrist flex | 360/4,096 degree (degrees) | 1092 | 1256 |
| wrist roll | 360/4,096 degree (degrees) | 1743 | 1795 |
| gripper | 0.08258 of LeRobot's calibrated scale (a range of 1211 counts) | 639 | 603 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (42,568 frames); every frame then read as it would arrive. The held-out half (40,938 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 46 | 68 | 9 |
| shoulder lift | 28 | 160 | 4 |
| elbow flex | 20 | 166 | 6 |
| wrist flex | 13 | 131 | 75 |
| wrist roll | 18 | 155 | 45 |
| gripper | 45 | 154 | 12 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
