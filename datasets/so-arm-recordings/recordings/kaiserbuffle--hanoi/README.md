# kaiserbuffle/hanoi

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [kaiserbuffle/hanoi](https://huggingface.co/datasets/kaiserbuffle/hanoi), revision `2c02c1adec39151f98f44a3ddba6c8a945130154` |
| Author | Matthias von Rakowski ([kaiserbuffle](https://huggingface.co/kaiserbuffle)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 50 episodes, 203,010 frames |
| Rank | 77 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 942 | 958 |
| shoulder lift | 360/4,096 degree (degrees) | 1949 | 1979 |
| elbow flex | 360/4,096 degree (degrees) | 2070 | 2110 |
| wrist flex | 360/4,096 degree (degrees) | 1614 | 1645 |
| wrist roll | 360/4,096 degree (degrees) | 1999 | 2231 |
| gripper | 0.08130 of LeRobot's calibrated scale (a range of 1230 counts) | 789 | 758 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (67,044 frames); every frame then read as it would arrive. The held-out half (135,966 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 37 | 64 | 4 |
| shoulder lift | 32 | 105 | 103 |
| elbow flex | 57 | 103 | 82 |
| wrist flex | 65 | 84 | 69 |
| wrist roll | 44 | 109 | 97 |
| gripper | 66 | 109 | 41 |

## The simulator against this arm

Not in the simulator test: this arm was recorded on another zero (a body joint's range is not centred inside the model's range; amendment 16), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
