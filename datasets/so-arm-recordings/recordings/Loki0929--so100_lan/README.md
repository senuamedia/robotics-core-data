# Loki0929/so100_lan

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [Loki0929/so100_lan](https://huggingface.co/datasets/Loki0929/so100_lan), revision `17852d65f6b871a62d760adce88e1dd899663585` |
| Author | Loki ([Loki0929](https://huggingface.co/Loki0929)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 800 episodes, 256,427 frames |
| Rank | 51 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 2328 | 2378 |
| shoulder lift | 360/4,096 degree (degrees) | 1997 | 1983 |
| elbow flex | 360/4,096 degree (degrees) | 2046 | 2101 |
| wrist flex | 360/4,096 degree (degrees) | 1900 | 1953 |
| wrist roll | 360/4,096 degree (degrees) | 1368 | 1394 |
| gripper | 0.08319 of LeRobot's calibrated scale (a range of 1202 counts) | 756 | 710 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (135,231 frames); every frame then read as it would arrive. The held-out half (120,813 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 55 | 162 | 16 |
| shoulder lift | 54 | 170 | 16 |
| elbow flex | 40 | 219 | 3 |
| wrist flex | 73 | 205 | 4 |
| wrist roll | 56 | 143 | 22 |
| gripper | 24 | 209 | 0 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
