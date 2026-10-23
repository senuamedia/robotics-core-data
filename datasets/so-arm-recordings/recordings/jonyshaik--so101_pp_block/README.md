# jonyshaik/so101_pp_block

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [jonyshaik/so101_pp_block](https://huggingface.co/datasets/jonyshaik/so101_pp_block), revision `1ab4b72579821ab2ce885fcbef9234e71103c575` |
| Author | [jonyshaik](https://huggingface.co/jonyshaik) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 51 episodes, 90,879 frames |
| Rank | 226 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 902 | 921 |
| shoulder lift | 360/4,096 degree (degrees) | 1941 | 1938 |
| elbow flex | 360/4,096 degree (degrees) | 2020 | 2069 |
| wrist flex | 360/4,096 degree (degrees) | 1223 | 1248 |
| wrist roll | 360/4,096 degree (degrees) | 2416 | 2448 |
| gripper | 0.07855 of LeRobot's calibrated scale (a range of 1273 counts) | 752 | 754 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,549 frames); every frame then read as it would arrive. The held-out half (46,330 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 30 | 62 | 7 |
| shoulder lift | 30 | 135 | 0 |
| elbow flex | 44 | 149 | 4 |
| wrist flex | 34 | 114 | 41 |
| wrist roll | 12 | 148 | 95 |
| gripper | 4 | 166 | 34 |

## The simulator against this arm

Not in the simulator test: this arm was recorded on another zero (a body joint's range is not centred inside the model's range; amendment 16), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
