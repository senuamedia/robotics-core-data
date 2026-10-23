# CrazyYhang/precision-assembly

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [CrazyYhang/precision-assembly](https://huggingface.co/datasets/CrazyYhang/precision-assembly), revision `615a13a463195e1dd95a05adde03e552ec3bff11` |
| Author | Yhang ([CrazyYhang](https://huggingface.co/CrazyYhang)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.0, 30 frames a second |
| Recording | 160 episodes, 165,660 frames |
| Rank | 103 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 593 | 634 |
| shoulder lift | 360/4,096 degree (degrees) | 1654 | 1629 |
| elbow flex | 360/4,096 degree (degrees) | 1588 | 1659 |
| wrist flex | 360/4,096 degree (degrees) | 734 | 796 |
| wrist roll | 360/4,096 degree (degrees) | 1244 | 1284 |
| gripper | 0.09017 of LeRobot's calibrated scale (a range of 1109 counts) | 659 | 742 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (88,245 frames); every frame then read as it would arrive. The held-out half (77,415 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 30 | 53 | 0 |
| shoulder lift | 43 | 82 | 23 |
| elbow flex | 29 | 86 | 36 |
| wrist flex | 42 | 53 | 0 |
| wrist roll | 51 | 74 | 8 |
| gripper | 33 | 67 | 0 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
