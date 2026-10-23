# observabot/so101_die_mat4

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [observabot/so101_die_mat4](https://huggingface.co/datasets/observabot/so101_die_mat4), revision `8fc24d3508c6f4c6cd49070d78db9b0e043641ba` |
| Author | Ob Bo ([observabot](https://huggingface.co/observabot)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 217 episodes, 119,028 frames |
| Rank | 151 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09187 of LeRobot's calibrated scale (a range of 2177 counts) | 1431 | 1770 |
| shoulder lift | 0.08375 of LeRobot's calibrated scale (a range of 2388 counts) | 2001 | 2032 |
| elbow flex | 0.08977 of LeRobot's calibrated scale (a range of 2228 counts) | 1988 | 2039 |
| wrist flex | 0.08460 of LeRobot's calibrated scale (a range of 2364 counts) | 1027 | 1041 |
| wrist roll | 0.05210 of LeRobot's calibrated scale (a range of 3839 counts) | 1417 | 1448 |
| gripper | 0.06540 of LeRobot's calibrated scale (a range of 1529 counts) | 756 | 685 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (64,292 frames); every frame then read as it would arrive. The held-out half (54,736 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 43 | 85 | 15 |
| shoulder lift | 50 | 119 | 12 |
| elbow flex | 47 | 105 | 96 |
| wrist flex | 52 | 81 | 18 |
| wrist roll | 32 | 93 | 10 |
| gripper | 30 | 132 | 50 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.334 | 0.430 | 1.119 | 0.762 |
| shoulder lift | 1.306 | 1.940 | 3.492 | 2.678 |
| elbow flex | 0.589 | 1.538 | 3.634 | 2.844 |
| wrist flex | 3.007 | 3.070 | 1.850 | 1.357 |
| wrist roll | 0.182 | 0.169 | 0.405 | 0.358 |
| gripper | 1.883 | 2.221 | 3.280 | 2.608 |
| five body joints | 1.499 | 1.776 | 2.459 | 1.887 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
