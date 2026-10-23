# Mariaceleste12/so101_pickplace_v4

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Mariaceleste12/so101_pickplace_v4](https://huggingface.co/datasets/Mariaceleste12/so101_pickplace_v4), revision `a325c800eee2c4bb1b3199474c1a1c575697adb8` |
| Author | Fernandez ([Mariaceleste12](https://huggingface.co/Mariaceleste12)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 167 episodes, 227,990 frames |
| Rank | 63 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1407 | 1441 |
| shoulder lift | 360/4,095 degree (degrees) | 1824 | 1897 |
| elbow flex | 360/4,095 degree (degrees) | 1578 | 1685 |
| wrist flex | 360/4,095 degree (degrees) | 2093 | 2195 |
| wrist roll | 360/4,095 degree (degrees) | 1117 | 1089 |
| gripper | 0.06645 of LeRobot's calibrated scale (a range of 1505 counts) | 950 | 838 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (148,942 frames); every frame then read as it would arrive. The held-out half (81,638 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 43 | 76 | 2427 |
| shoulder lift | 49 | 87 | 1929 |
| elbow flex | 55 | 76 | 2327 |
| wrist flex | 46 | 123 | 1965 |
| wrist roll | 55 | 109 | 1392 |
| gripper | 15 | 100 | 1352 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 94.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 2.561 | 3.592 | 5.205 | 4.751 |
| shoulder lift | 3.618 | 3.538 | 0.890 | 0.818 |
| elbow flex | 2.713 | 2.758 | 1.311 | 1.311 |
| wrist flex | 0.581 | 0.590 | 0.682 | 0.678 |
| wrist roll | 0.783 | 0.975 | 1.223 | 1.108 |
| gripper | 0.511 | 0.512 | 0.781 | 0.781 |
| five body joints | 2.364 | 2.620 | 2.512 | 2.308 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
