# Bixymax/so101_dataset_all_shapes_v2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Bixymax/so101_dataset_all_shapes_v2](https://huggingface.co/datasets/Bixymax/so101_dataset_all_shapes_v2), revision `40e2a392b149e2751d3767666681552b91fb5bbf` |
| Author | [Bixymax](https://huggingface.co/Bixymax) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 320 episodes, 117,539 frames |
| Rank | 156 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1609 | 1628 |
| shoulder lift | 360/4,095 degree (degrees) | 2042 | 2045 |
| elbow flex | 360/4,095 degree (degrees) | 2066 | 2102 |
| wrist flex | 360/4,095 degree (degrees) | 2316 | 2328 |
| wrist roll | 360/4,095 degree (degrees) | 2109 | 2105 |
| gripper | 0.06293 of LeRobot's calibrated scale (a range of 1589 counts) | 1364 | 1081 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (56,292 frames); every frame then read as it would arrive. The held-out half (61,247 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 47 | 149 | 12 |
| shoulder lift | 64 | 194 | 24 |
| elbow flex | 58 | 216 | 17 |
| wrist flex | 31 | 307 | 21 |
| wrist roll | 55 | 241 | 5 |
| gripper | 27 | 140 | 7 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.513 | 1.387 | 3.052 | 2.030 |
| shoulder lift | 1.781 | 2.595 | 4.376 | 3.083 |
| elbow flex | 1.275 | 2.482 | 5.206 | 3.788 |
| wrist flex | 1.182 | 3.280 | 6.714 | 4.708 |
| wrist roll | 0.911 | 1.691 | 3.717 | 2.445 |
| gripper | 9.123 | 9.118 | 10.192 | 9.776 |
| five body joints | 1.207 | 2.385 | 4.784 | 3.350 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
