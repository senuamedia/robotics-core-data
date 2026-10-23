# open-cloth/so101-full-fold-merged

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [open-cloth/so101-full-fold-merged](https://huggingface.co/datasets/open-cloth/so101-full-fold-merged), revision `098156104900bdb9c47ad92d7d851cbc251e0e47` |
| Author | [open-cloth](https://huggingface.co/open-cloth) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 118 episodes, 105,071 frames |
| Rank | 184 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1519 | 1533 |
| shoulder lift | 360/4,095 degree (degrees) | 1624 | 1598 |
| elbow flex | 360/4,095 degree (degrees) | 2033 | 2070 |
| wrist flex | 360/4,095 degree (degrees) | 1096 | 1108 |
| wrist roll | 360/4,095 degree (degrees) | 387 | 1044 |
| gripper | 0.06321 of LeRobot's calibrated scale (a range of 1582 counts) | 478 | 465 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (55,254 frames); every frame then read as it would arrive. The held-out half (49,817 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 45 | 95 | 53 |
| shoulder lift | 78 | 135 | 24 |
| elbow flex | 44 | 140 | 31 |
| wrist flex | 54 | 116 | 15 |
| wrist roll | 15 | 41 | 6 |
| gripper | 17 | 115 | 15 |

## The simulator against this arm

Not in the simulator test: this arm was recorded on another zero (a body joint's range is not centred inside the model's range; amendment 16), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
