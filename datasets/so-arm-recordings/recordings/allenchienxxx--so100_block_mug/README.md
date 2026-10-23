# allenchienxxx/so100_block_mug

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [allenchienxxx/so100_block_mug](https://huggingface.co/datasets/allenchienxxx/so100_block_mug), revision `2cfe4bbab325d60164b00539738ca5c2d3c83a3c` |
| Author | [allenchienxxx](https://huggingface.co/allenchienxxx) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 101 episodes, 90,285 frames |
| Rank | 229 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1357 | 1365 |
| shoulder lift | 360/4,096 degree (degrees) | 1799 | 1791 |
| elbow flex | 360/4,096 degree (degrees) | 2033 | 2042 |
| wrist flex | 360/4,096 degree (degrees) | 805 | 811 |
| wrist roll | 360/4,096 degree (degrees) | 886 | 930 |
| gripper | 0.08032 of LeRobot's calibrated scale (a range of 1245 counts) | 915 | 862 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,684 frames); every frame then read as it would arrive. The held-out half (45,601 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 28 | 84 | 5 |
| shoulder lift | 11 | 182 | 4 |
| elbow flex | 10 | 170 | 14 |
| wrist flex | 33 | 110 | 31 |
| wrist roll | 33 | 104 | 4 |
| gripper | 2 | 222 | 3 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
