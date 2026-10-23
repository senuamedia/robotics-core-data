# LeeAfe/so101-megamix-v1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [LeeAfe/so101-megamix-v1](https://huggingface.co/datasets/LeeAfe/so101-megamix-v1), revision `dbccad3b8069ff1f868569482e159109b2cd3eee` |
| Author | Lee Afeawo ([LeeAfe](https://huggingface.co/LeeAfe)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 400 episodes, 179,166 frames |
| Rank | 91 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09452 of LeRobot's calibrated scale (a range of 2116 counts) | 1819 | 1982 |
| shoulder lift | 0.08536 of LeRobot's calibrated scale (a range of 2343 counts) | 1884 | 1907 |
| elbow flex | 0.09079 of LeRobot's calibrated scale (a range of 2203 counts) | 1792 | 1856 |
| wrist flex | 0.08718 of LeRobot's calibrated scale (a range of 2294 counts) | 780 | 799 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1758 | 1659 |
| gripper | 0.06859 of LeRobot's calibrated scale (a range of 1458 counts) | 831 | 724 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (62,306 frames); every frame then read as it would arrive. The held-out half (116,860 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 45 | 162 | 124 |
| shoulder lift | 65 | 142 | 0 |
| elbow flex | 58 | 123 | 15 |
| wrist flex | 43 | 152 | 46 |
| wrist roll | 29 | 132 | 6 |
| gripper | 37 | 274 | 2 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.278 | 0.461 | 1.045 | 0.776 |
| shoulder lift | 1.535 | 2.447 | 4.440 | 3.284 |
| elbow flex | 1.035 | 1.785 | 4.044 | 3.359 |
| wrist flex | 0.363 | 0.472 | 0.903 | 0.697 |
| wrist roll | 0.246 | 0.292 | 0.782 | 0.590 |
| gripper | 6.229 | 6.375 | 7.170 | 6.756 |
| five body joints | 0.859 | 1.393 | 2.778 | 2.168 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
