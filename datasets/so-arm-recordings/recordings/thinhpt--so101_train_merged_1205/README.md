# thinhpt/so101_train_merged_1205

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [thinhpt/so101_train_merged_1205](https://huggingface.co/datasets/thinhpt/so101_train_merged_1205), revision `756eb0678a0b56585635656a5d09ab9a34c897ba` |
| Author | [thinhpt](https://huggingface.co/thinhpt) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 78,598 frames |
| Rank | 273 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07358 of LeRobot's calibrated scale (a range of 2718 counts) | 1194 | 1214 |
| shoulder lift | 0.08410 of LeRobot's calibrated scale (a range of 2378 counts) | 1736 | 1754 |
| elbow flex | 0.09506 of LeRobot's calibrated scale (a range of 2104 counts) | 1560 | 1717 |
| wrist flex | 0.08861 of LeRobot's calibrated scale (a range of 2257 counts) | 580 | 614 |
| wrist roll | 0.05277 of LeRobot's calibrated scale (a range of 3790 counts) | 1142 | 1176 |
| gripper | 0.06662 of LeRobot's calibrated scale (a range of 1501 counts) | 626 | 558 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (41,507 frames); every frame then read as it would arrive. The held-out half (37,091 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 48 | 107 | 0 |
| shoulder lift | 44 | 131 | 60 |
| elbow flex | 64 | 137 | 102 |
| wrist flex | 41 | 93 | 49 |
| wrist roll | 27 | 105 | 2 |
| gripper | 19 | 116 | 0 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.434 | 0.621 | 1.640 | 1.016 |
| shoulder lift | 1.250 | 1.797 | 3.303 | 2.187 |
| elbow flex | 1.862 | 2.105 | 3.188 | 2.361 |
| wrist flex | 0.471 | 0.568 | 1.604 | 1.071 |
| wrist roll | 0.349 | 0.319 | 0.981 | 0.624 |
| gripper | 1.427 | 1.553 | 2.851 | 2.118 |
| five body joints | 1.054 | 1.301 | 2.337 | 1.608 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
