# LePixelArt/all_datasets_aggregated

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [LePixelArt/all_datasets_aggregated](https://huggingface.co/datasets/LePixelArt/all_datasets_aggregated), revision `c5f6f75745de20e319d94951f001102a766ea558` |
| Author | [LePixelArt](https://huggingface.co/LePixelArt) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 266 episodes, 127,021 frames |
| Rank | 137 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08881 of LeRobot's calibrated scale (a range of 2252 counts) | 1480 | 1315 |
| shoulder lift | 0.08288 of LeRobot's calibrated scale (a range of 2413 counts) | 1734 | 1714 |
| elbow flex | 0.08662 of LeRobot's calibrated scale (a range of 2309 counts) | 1668 | 1715 |
| wrist flex | 0.08457 of LeRobot's calibrated scale (a range of 2365 counts) | 2315 | 2392 |
| wrist roll | 0.05133 of LeRobot's calibrated scale (a range of 3896 counts) | 1349 | 1398 |
| gripper | 0.15898 of LeRobot's calibrated scale (a range of 629 counts) | 604 | 800 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (68,754 frames); every frame then read as it would arrive. The held-out half (58,267 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 32 | 130 | 11 |
| shoulder lift | 31 | 149 | 65 |
| elbow flex | 30 | 144 | 48 |
| wrist flex | 42 | 153 | 1159 |
| wrist roll | 11 | 181 | 2 |
| gripper | 37 | 134 | 9 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.381 | 1.117 | 2.379 | 1.644 |
| shoulder lift | 1.459 | 2.682 | 4.623 | 3.436 |
| elbow flex | 1.671 | 2.683 | 4.710 | 3.551 |
| wrist flex | 0.463 | 1.154 | 2.366 | 1.671 |
| wrist roll | 1.070 | 2.339 | 4.000 | 2.916 |
| gripper | 2.646 | 2.891 | 3.596 | 3.042 |
| five body joints | 1.133 | 2.118 | 3.763 | 2.772 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
