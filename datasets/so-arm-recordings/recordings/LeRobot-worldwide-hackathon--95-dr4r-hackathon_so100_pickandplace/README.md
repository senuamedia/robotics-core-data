# LeRobot-worldwide-hackathon/95-dr4r-hackathon_so100_pickandplace

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [LeRobot-worldwide-hackathon/95-dr4r-hackathon_so100_pickandplace](https://huggingface.co/datasets/LeRobot-worldwide-hackathon/95-dr4r-hackathon_so100_pickandplace), revision `565841f0c32ee7b7d9e1433f4d96c8ba24c7ae4f` |
| Author | LeRobot Worldwide Hackathon ([LeRobot-worldwide-hackathon](https://huggingface.co/LeRobot-worldwide-hackathon)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.0, 30 frames a second |
| Recording | 164 episodes, 144,792 frames |
| Rank | 117 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1318 | 1343 |
| shoulder lift | 360/4,096 degree (degrees) | 1659 | 1650 |
| elbow flex | 360/4,096 degree (degrees) | 1790 | 1877 |
| wrist flex | 360/4,096 degree (degrees) | 953 | 957 |
| wrist roll | 360/4,096 degree (degrees) | 2354 | 2413 |
| gripper | 0.08803 of LeRobot's calibrated scale (a range of 1136 counts) | 811 | 812 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (71,649 frames); every frame then read as it would arrive. The held-out half (73,143 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 93 | 107 | 36 |
| shoulder lift | 40 | 138 | 2 |
| elbow flex | 67 | 160 | 0 |
| wrist flex | 83 | 132 | 0 |
| wrist roll | 78 | 140 | 73 |
| gripper | 44 | 208 | 4 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
