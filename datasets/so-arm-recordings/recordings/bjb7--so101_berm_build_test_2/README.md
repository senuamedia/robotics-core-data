# bjb7/so101_berm_build_test_2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [bjb7/so101_berm_build_test_2](https://huggingface.co/datasets/bjb7/so101_berm_build_test_2), revision `07cfb644bf038fe0d9ebcb2485e8337d2a094fd1` |
| Author | [bjb7](https://huggingface.co/bjb7) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 50 episodes, 74,683 frames |
| Rank | 293 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1080 | 1099 |
| shoulder lift | 360/4,096 degree (degrees) | 2279 | 2298 |
| elbow flex | 360/4,096 degree (degrees) | 2171 | 2187 |
| wrist flex | 360/4,096 degree (degrees) | 1352 | 1415 |
| wrist roll | 360/4,096 degree (degrees) | 1272 | 1304 |
| gripper | 0.07013 of LeRobot's calibrated scale (a range of 1426 counts) | 9 | 18 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (37,613 frames); every frame then read as it would arrive. The held-out half (37,070 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 39 | 86 | 10 |
| shoulder lift | 54 | 145 | 11 |
| elbow flex | 49 | 155 | 30 |
| wrist flex | 27 | 131 | 12 |
| wrist roll | 29 | 113 | 15 |
| gripper | 3 | 5 | 0 |

## The simulator against this arm

Not in the simulator test: this arm was recorded on another zero (a body joint's range is not centred inside the model's range; amendment 16), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
