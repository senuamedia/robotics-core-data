# Chojins/chess_game_009_white

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [Chojins/chess_game_009_white](https://huggingface.co/datasets/Chojins/chess_game_009_white), revision `9da8e6d4569f4fb49552730c7483b59d78816dd9` |
| Author | Jacob Gertsch ([Chojins](https://huggingface.co/Chojins)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 342 episodes, 140,709 frames |
| Rank | 123 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1100 | 1130 |
| shoulder lift | 360/4,096 degree (degrees) | 1583 | 1558 |
| elbow flex | 360/4,096 degree (degrees) | 1304 | 1364 |
| wrist flex | 360/4,096 degree (degrees) | 991 | 1006 |
| wrist roll | 360/4,096 degree (degrees) | 1674 | 2179 |
| gripper | 0.07559 of LeRobot's calibrated scale (a range of 1323 counts) | 289 | 250 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (73,628 frames); every frame then read as it would arrive. The held-out half (67,081 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 46 | 61 | 18 |
| shoulder lift | 54 | 88 | 5 |
| elbow flex | 54 | 77 | 10 |
| wrist flex | 54 | 77 | 20 |
| wrist roll | 61 | 93 | 21 |
| gripper | 41 | 75 | 0 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
