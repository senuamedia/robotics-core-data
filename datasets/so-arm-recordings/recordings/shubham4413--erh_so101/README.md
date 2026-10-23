# shubham4413/erh_so101

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [shubham4413/erh_so101](https://huggingface.co/datasets/shubham4413/erh_so101), revision `250dd4ca283ed3f8c0e835105a7f2bbf4def7e92` |
| Author | Shubham nagar ([shubham4413](https://huggingface.co/shubham4413)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 300 episodes, 255,518 frames |
| Rank | 52 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1634 | 1996 |
| shoulder lift | 360/4,095 degree (degrees) | 2122 | 2097 |
| elbow flex | 360/4,095 degree (degrees) | 2169 | 2192 |
| wrist flex | 360/4,095 degree (degrees) | 1274 | 1299 |
| wrist roll | 360/4,095 degree (degrees) | 1901 | 1910 |
| gripper | 0.06707 of LeRobot's calibrated scale (a range of 1491 counts) | 1051 | 888 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (117,753 frames); every frame then read as it would arrive. The held-out half (137,765 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 52 | 100 | 1 |
| shoulder lift | 55 | 103 | 0 |
| elbow flex | 44 | 92 | 1 |
| wrist flex | 51 | 90 | 7 |
| wrist roll | 43 | 95 | 2 |
| gripper | 29 | 113 | 9 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
