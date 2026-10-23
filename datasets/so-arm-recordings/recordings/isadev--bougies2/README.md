# isadev/bougies2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [isadev/bougies2](https://huggingface.co/datasets/isadev/bougies2), revision `04cb81231cec6404717ad62d927563923bd57438` |
| Author | [isadev](https://huggingface.co/isadev) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 200 episodes, 85,934 frames |
| Rank | 248 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1014 | 1026 |
| shoulder lift | 360/4,096 degree (degrees) | 2033 | 2055 |
| elbow flex | 360/4,096 degree (degrees) | 2170 | 2201 |
| wrist flex | 360/4,096 degree (degrees) | 1588 | 1610 |
| wrist roll | 360/4,096 degree (degrees) | 2275 | 2302 |
| gripper | 0.06878 of LeRobot's calibrated scale (a range of 1454 counts) | 588 | 509 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (47,008 frames); every frame then read as it would arrive. The held-out half (38,926 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 46 | 61 | 16 |
| shoulder lift | 45 | 127 | 8 |
| elbow flex | 62 | 125 | 10 |
| wrist flex | 50 | 122 | 45 |
| wrist roll | 37 | 133 | 27 |
| gripper | 22 | 167 | 3 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
