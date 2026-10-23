# denys1912/so101_cube_in_box_v2_20260803_193625

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [denys1912/so101_cube_in_box_v2_20260803_193625](https://huggingface.co/datasets/denys1912/so101_cube_in_box_v2_20260803_193625), revision `fdce6eb529bb0367624bdc5d65650d4981825c46` |
| Author | [denys1912](https://huggingface.co/denys1912) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 260 episodes, 112,045 frames |
| Rank | 167 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1765 | 1792 |
| shoulder lift | 360/4,095 degree (degrees) | 2147 | 2155 |
| elbow flex | 360/4,095 degree (degrees) | 2269 | 2323 |
| wrist flex | 360/4,095 degree (degrees) | 1563 | 1660 |
| wrist roll | 360/4,095 degree (degrees) | 1840 | 1892 |
| gripper | 0.06807 of LeRobot's calibrated scale (a range of 1469 counts) | 1175 | 1036 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (57,902 frames); every frame then read as it would arrive. The held-out half (54,143 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 26 | 122 | 5 |
| shoulder lift | 57 | 151 | 0 |
| elbow flex | 31 | 133 | 10 |
| wrist flex | 49 | 151 | 0 |
| wrist roll | 28 | 129 | 6 |
| gripper | 22 | 96 | 19 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
