# escape112/so101_three_ball_order_v1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [escape112/so101_three_ball_order_v1](https://huggingface.co/datasets/escape112/so101_three_ball_order_v1), revision `9a8bdd8a16ce7d889b006057bf5133e6af9c9ac6` |
| Author | Lin ([escape112](https://huggingface.co/escape112)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 192 episodes, 148,371 frames |
| Rank | 112 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07785 of LeRobot's calibrated scale (a range of 2569 counts) | 1051 | 1077 |
| shoulder lift | 0.08351 of LeRobot's calibrated scale (a range of 2395 counts) | 1668 | 1663 |
| elbow flex | 0.09103 of LeRobot's calibrated scale (a range of 2197 counts) | 1603 | 1666 |
| wrist flex | 0.08511 of LeRobot's calibrated scale (a range of 2350 counts) | 759 | 799 |
| wrist roll | 0.05212 of LeRobot's calibrated scale (a range of 3837 counts) | 332 | 352 |
| gripper | 0.05784 of LeRobot's calibrated scale (a range of 1729 counts) | 696 | 561 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (77,063 frames); every frame then read as it would arrive. The held-out half (71,308 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 32 | 163 | 0 |
| shoulder lift | 26 | 206 | 17 |
| elbow flex | 27 | 189 | 6 |
| wrist flex | 51 | 131 | 13 |
| wrist roll | 30 | 72 | 2 |
| gripper | 24 | 163 | 2 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
