# dragon-95/so100_sorting_2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [dragon-95/so100_sorting_2](https://huggingface.co/datasets/dragon-95/so100_sorting_2), revision `a71a18c9566aebf1fe169b452f31397161a46ae6` |
| Author | he yixuan ([dragon-95](https://huggingface.co/dragon-95)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.0, 30 frames a second |
| Recording | 220 episodes, 169,922 frames |
| Rank | 98 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1225 | 1312 |
| shoulder lift | 360/4,096 degree (degrees) | 1842 | 1838 |
| elbow flex | 360/4,096 degree (degrees) | 2006 | 2028 |
| wrist flex | 360/4,096 degree (degrees) | 1166 | 1193 |
| wrist roll | 360/4,096 degree (degrees) | 2082 | 2110 |
| gripper | 0.08503 of LeRobot's calibrated scale (a range of 1176 counts) | 862 | 732 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (65,767 frames); every frame then read as it would arrive. The held-out half (104,155 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 51 | 127 | 39 |
| shoulder lift | 49 | 148 | 93 |
| elbow flex | 69 | 197 | 61 |
| wrist flex | 47 | 140 | 4 |
| wrist roll | 56 | 206 | 2 |
| gripper | 22 | 200 | 19 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
