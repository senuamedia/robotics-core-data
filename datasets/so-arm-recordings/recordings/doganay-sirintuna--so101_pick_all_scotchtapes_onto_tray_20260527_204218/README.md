# doganay-sirintuna/so101_pick_all_scotchtapes_onto_tray_20260527_204218

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [doganay-sirintuna/so101_pick_all_scotchtapes_onto_tray_20260527_204218](https://huggingface.co/datasets/doganay-sirintuna/so101_pick_all_scotchtapes_onto_tray_20260527_204218), revision `212314910be6eac2edb519dbb64d76d8ae195af7` |
| Author | [doganay-sirintuna](https://huggingface.co/doganay-sirintuna) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 50 episodes, 93,529 frames |
| Rank | 218 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1414 | 1444 |
| shoulder lift | 360/4,095 degree (degrees) | 1987 | 1993 |
| elbow flex | 360/4,095 degree (degrees) | 2141 | 2184 |
| wrist flex | 360/4,095 degree (degrees) | 1082 | 1123 |
| wrist roll | 360/4,095 degree (degrees) | 1167 | 1202 |
| gripper | 0.06780 of LeRobot's calibrated scale (a range of 1475 counts) | 899 | 793 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (50,594 frames); every frame then read as it would arrive. The held-out half (42,935 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 60 | 89 | 24 |
| shoulder lift | 46 | 112 | 42 |
| elbow flex | 51 | 119 | 50 |
| wrist flex | 52 | 101 | 6 |
| wrist roll | 32 | 96 | 16 |
| gripper | 26 | 119 | 2 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
