# rslxcvg/banana_act_direct_color_simple_v1_molmo_compat

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [rslxcvg/banana_act_direct_color_simple_v1_molmo_compat](https://huggingface.co/datasets/rslxcvg/banana_act_direct_color_simple_v1_molmo_compat), revision `7c0a1397cbba7fed9a685b875fe31bfc2021ee62` |
| Author | [rslxcvg](https://huggingface.co/rslxcvg) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 360 episodes, 150,084 frames |
| Rank | 108 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1061 | 1069 |
| shoulder lift | 360/4,095 degree (degrees) | 1080 | 1081 |
| elbow flex | 360/4,095 degree (degrees) | 948 | 960 |
| wrist flex | 360/4,095 degree (degrees) | 533 | 537 |
| wrist roll | 360/4,095 degree (degrees) | 485 | 468 |
| gripper | 0.06878 of LeRobot's calibrated scale (a range of 1454 counts) | 528 | 448 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (72,804 frames); every frame then read as it would arrive. The held-out half (77,280 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 48 | 64 | 0 |
| shoulder lift | 41 | 73 | 84 |
| elbow flex | 48 | 83 | 60 |
| wrist flex | 34 | 47 | 78 |
| wrist roll | 24 | 64 | 126 |
| gripper | 39 | 86 | 78 |

## The simulator against this arm

Not in the simulator test: this arm was recorded on another zero (a body joint's range is not centred inside the model's range; amendment 16), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
