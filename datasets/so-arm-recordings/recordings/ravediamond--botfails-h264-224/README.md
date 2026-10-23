# ravediamond/botfails-h264-224

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [ravediamond/botfails-h264-224](https://huggingface.co/datasets/ravediamond/botfails-h264-224), revision `6859130e88561ec64a6545618683b3d63e088858` |
| Author | ravindu somawansa ([ravediamond](https://huggingface.co/ravediamond)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Built from (as its card credits) | [kantine/BotFails](https://huggingface.co/datasets/kantine/BotFails), by quentin (kantine), apache-2.0 |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 323 episodes, 414,359 frames |
| Rank | 30 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1930 | 2025 |
| shoulder lift | 360/4,096 degree (degrees) | 2189 | 2331 |
| elbow flex | 360/4,096 degree (degrees) | 2203 | 2218 |
| wrist flex | 360/4,096 degree (degrees) | 2444 | 2453 |
| wrist roll | 360/4,096 degree (degrees) | 2892 | 3366 |
| gripper | 0.08666 of LeRobot's calibrated scale (a range of 1154 counts) | 1192 | 1064 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (215,863 frames); every frame then read as it would arrive. The held-out half (198,496 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 102 | 162 | 8 |
| shoulder lift | 84 | 189 | 33 |
| elbow flex | 100 | 178 | 80 |
| wrist flex | 91 | 168 | 101 |
| wrist roll | 93 | 236 | 44 |
| gripper | 21 | 216 | 10 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
