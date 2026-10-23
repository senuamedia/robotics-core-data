# Rorschach4153/so101_60_new

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [Rorschach4153/so101_60_new](https://huggingface.co/datasets/Rorschach4153/so101_60_new), revision `2fb98415dffcc50a48f7d221fd3faeb674a77e44` |
| Author | Rorschach233 ([Rorschach4153](https://huggingface.co/Rorschach4153)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101` |
| Format | LeRobot v2.1, 60 frames a second |
| Recording | 115 episodes, 155,662 frames |
| Rank | 106 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1402 | 1408 |
| shoulder lift | 360/4,096 degree (degrees) | 1960 | 1964 |
| elbow flex | 360/4,096 degree (degrees) | 1885 | 1919 |
| wrist flex | 360/4,096 degree (degrees) | 1912 | 1934 |
| wrist roll | 360/4,096 degree (degrees) | 1941 | 1987 |
| gripper | 0.06892 of LeRobot's calibrated scale (a range of 1451 counts) | 584 | 483 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (76,652 frames); every frame then read as it would arrive. The held-out half (79,010 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 22 | 38 | 1 |
| shoulder lift | 45 | 58 | 3 |
| elbow flex | 36 | 49 | 10 |
| wrist flex | 48 | 63 | 11 |
| wrist roll | 43 | 45 | 57 |
| gripper | 19 | 58 | 22 |

## The simulator against this arm

Not in the simulator test: this arm was recorded on another zero (a body joint's range is not centred inside the model's range; amendment 16), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
