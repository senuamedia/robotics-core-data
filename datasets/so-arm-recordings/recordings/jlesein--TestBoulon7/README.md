# jlesein/TestBoulon7

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [jlesein/TestBoulon7](https://huggingface.co/datasets/jlesein/TestBoulon7), revision `7880fad2babdd4ca3c3b5111970469aee797dab9` |
| Author | [jlesein](https://huggingface.co/jlesein) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 400 episodes, 105,700 frames |
| Rank | 181 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1537 | 1550 |
| shoulder lift | 360/4,096 degree (degrees) | 2174 | 2201 |
| elbow flex | 360/4,096 degree (degrees) | 2158 | 2196 |
| wrist flex | 360/4,096 degree (degrees) | 1141 | 1165 |
| wrist roll | 360/4,096 degree (degrees) | 1943 | 1973 |
| gripper | 0.06998 of LeRobot's calibrated scale (a range of 1429 counts) | 584 | 428 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (52,318 frames); every frame then read as it would arrive. The held-out half (53,382 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 74 | 121 | 1 |
| shoulder lift | 23 | 232 | 4 |
| elbow flex | 16 | 223 | 7 |
| wrist flex | 59 | 118 | 23 |
| wrist roll | 37 | 122 | 59 |
| gripper | 27 | 168 | 1 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
