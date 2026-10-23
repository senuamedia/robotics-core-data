# 3b4t4/so101_pickplace_3color_20260601_135201

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [3b4t4/so101_pickplace_3color_20260601_135201](https://huggingface.co/datasets/3b4t4/so101_pickplace_3color_20260601_135201), revision `a8e9aaf5195e6d7d52bee6921df4b24f0c3d2fc7` |
| Author | [3b4t4](https://huggingface.co/3b4t4) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 150 episodes, 74,729 frames |
| Rank | 292 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1087 | 1093 |
| shoulder lift | 360/4,095 degree (degrees) | 2112 | 2119 |
| elbow flex | 360/4,095 degree (degrees) | 2180 | 2218 |
| wrist flex | 360/4,095 degree (degrees) | 1082 | 1091 |
| wrist roll | 360/4,095 degree (degrees) | 889 | 924 |
| gripper | 0.07353 of LeRobot's calibrated scale (a range of 1360 counts) | 529 | 528 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (40,464 frames); every frame then read as it would arrive. The held-out half (34,265 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 27 | 95 | 158 |
| shoulder lift | 67 | 109 | 120 |
| elbow flex | 27 | 130 | 11 |
| wrist flex | 44 | 105 | 9 |
| wrist roll | 36 | 90 | 1 |
| gripper | 17 | 113 | 0 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
