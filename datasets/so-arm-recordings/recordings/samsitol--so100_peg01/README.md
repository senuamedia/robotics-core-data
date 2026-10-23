# samsitol/so100_peg01

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [samsitol/so100_peg01](https://huggingface.co/datasets/samsitol/so100_peg01), revision `540f9f3cf9c8c09704f11a739848823c2e12e693` |
| Author | Sam ([samsitol](https://huggingface.co/samsitol)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 150 episodes, 133,461 frames |
| Rank | 131 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1809 | 1815 |
| shoulder lift | 360/4,096 degree (degrees) | 1928 | 1910 |
| elbow flex | 360/4,096 degree (degrees) | 2103 | 2141 |
| wrist flex | 360/4,096 degree (degrees) | 961 | 967 |
| wrist roll | 360/4,096 degree (degrees) | 2110 | 2149 |
| gripper | 0.09615 of LeRobot's calibrated scale (a range of 1040 counts) | 924 | 669 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (66,732 frames); every frame then read as it would arrive. The held-out half (66,729 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 79 | 3 |
| shoulder lift | 43 | 117 | 16 |
| elbow flex | 25 | 108 | 23 |
| wrist flex | 50 | 87 | 10 |
| wrist roll | 73 | 93 | 13 |
| gripper | 45 | 124 | 50 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
