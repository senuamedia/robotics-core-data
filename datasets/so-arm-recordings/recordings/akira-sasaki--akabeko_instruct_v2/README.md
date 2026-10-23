# akira-sasaki/akabeko_instruct_v2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [akira-sasaki/akabeko_instruct_v2](https://huggingface.co/datasets/akira-sasaki/akabeko_instruct_v2), revision `120c14cb9111a1e4db206785578be12c2b4dced8` |
| Author | [akira-sasaki](https://huggingface.co/akira-sasaki) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 345 episodes, 103,498 frames |
| Rank | 193 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 628 | 627 |
| shoulder lift | 360/4,096 degree (degrees) | 1797 | 1864 |
| elbow flex | 360/4,096 degree (degrees) | 1988 | 2055 |
| wrist flex | 360/4,096 degree (degrees) | 868 | 888 |
| wrist roll | 360/4,096 degree (degrees) | 742 | 765 |
| gripper | 0.06964 of LeRobot's calibrated scale (a range of 1436 counts) | 492 | 539 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (51,600 frames); every frame then read as it would arrive. The held-out half (51,898 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 92 | 3 |
| shoulder lift | 46 | 154 | 15 |
| elbow flex | 34 | 158 | 19 |
| wrist flex | 35 | 99 | 21 |
| wrist roll | 25 | 84 | 19 |
| gripper | 21 | 152 | 1 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
