# Cornito/so101_chess17

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [Cornito/so101_chess17](https://huggingface.co/datasets/Cornito/so101_chess17), revision `e28ff179f5a99a9b2ad9c7ff93783a79312bb714` |
| Author | Corneille Marechal ([Cornito](https://huggingface.co/Cornito)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 190 episodes, 137,956 frames |
| Rank | 128 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 991 | 1009 |
| shoulder lift | 360/4,096 degree (degrees) | 1734 | 1733 |
| elbow flex | 360/4,096 degree (degrees) | 1728 | 1793 |
| wrist flex | 360/4,096 degree (degrees) | 2273 | 2291 |
| wrist roll | 360/4,096 degree (degrees) | 822 | 846 |
| gripper | 0.07057 of LeRobot's calibrated scale (a range of 1417 counts) | 294 | 290 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (70,279 frames); every frame then read as it would arrive. The held-out half (67,677 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 44 | 59 | 14 |
| shoulder lift | 53 | 92 | 4 |
| elbow flex | 49 | 77 | 22 |
| wrist flex | 30 | 129 | 13 |
| wrist roll | 13 | 52 | 6 |
| gripper | 12 | 70 | 0 |

## The simulator against this arm

Not in the simulator test: this arm was recorded on another zero (a body joint's range is not centred inside the model's range; amendment 16), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
