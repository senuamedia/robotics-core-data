# charleyong/so100_tic_tac_toe_autoend

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [charleyong/so100_tic_tac_toe_autoend](https://huggingface.co/datasets/charleyong/so100_tic_tac_toe_autoend), revision `7601cb6353bfb19faf613d2d5f778157ca4ba701` |
| Author | [charleyong](https://huggingface.co/charleyong) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 180 episodes, 74,377 frames |
| Rank | 296 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 1516 | 1533 |
| shoulder lift | 360/4,096 degree (degrees) | 1857 | 1868 |
| elbow flex | 360/4,096 degree (degrees) | 1739 | 1798 |
| wrist flex | 360/4,096 degree (degrees) | 1222 | 1239 |
| wrist roll | 360/4,096 degree (degrees) | 899 | 902 |
| gripper | 0.06940 of LeRobot's calibrated scale (a range of 1441 counts) | 441 | 330 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (42,065 frames); every frame then read as it would arrive. The held-out half (32,312 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 23 | 205 | 274 |
| shoulder lift | 48 | 140 | 132 |
| elbow flex | 22 | 151 | 68 |
| wrist flex | 38 | 97 | 95 |
| wrist roll | 47 | 75 | 33 |
| gripper | 21 | 125 | 36 |

## The simulator against this arm

Not in the simulator test: the robot type is not a LeRobot follower of the current calibration (the model's zero), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
