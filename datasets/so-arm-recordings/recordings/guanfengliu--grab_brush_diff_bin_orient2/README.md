# guanfengliu/grab_brush_diff_bin_orient2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [guanfengliu/grab_brush_diff_bin_orient2](https://huggingface.co/datasets/guanfengliu/grab_brush_diff_bin_orient2), revision `028557af09333fdf2ce5cfc2b5ae269feead498e` |
| Author | [guanfengliu](https://huggingface.co/guanfengliu) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 80 episodes, 95,162 frames |
| Rank | 211 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,096 degree (degrees) | 2131 | 2176 |
| shoulder lift | 360/4,096 degree (degrees) | 1315 | 1323 |
| elbow flex | 360/4,096 degree (degrees) | 1728 | 1813 |
| wrist flex | 360/4,096 degree (degrees) | 695 | 890 |
| wrist roll | 360/4,096 degree (degrees) | 2408 | 2485 |
| gripper | 0.08326 of LeRobot's calibrated scale (a range of 1201 counts) | 755 | 768 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (47,627 frames); every frame then read as it would arrive. The held-out half (47,535 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 36 | 102 | 32 |
| shoulder lift | 29 | 98 | 1 |
| elbow flex | 28 | 99 | 22 |
| wrist flex | 34 | 63 | 26 |
| wrist roll | 10 | 120 | 15 |
| gripper | 15 | 125 | 24 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
