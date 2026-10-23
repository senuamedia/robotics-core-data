# BrutalCaesar/phi_so101_cubes_cylinder_recovery_v1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [BrutalCaesar/phi_so101_cubes_cylinder_recovery_v1](https://huggingface.co/datasets/BrutalCaesar/phi_so101_cubes_cylinder_recovery_v1), revision `cf44c0993c77e89629fd1e598088eef8187845c9` |
| Author | [BrutalCaesar](https://huggingface.co/BrutalCaesar) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 143 episodes, 81,943 frames |
| Rank | 261 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 891 | 909 |
| shoulder lift | 360/4,095 degree (degrees) | 1964 | 2001 |
| elbow flex | 360/4,095 degree (degrees) | 2140 | 2179 |
| wrist flex | 360/4,095 degree (degrees) | 1902 | 1904 |
| wrist roll | 360/4,095 degree (degrees) | 1682 | 1710 |
| gripper | 0.06614 of LeRobot's calibrated scale (a range of 1512 counts) | 821 | 680 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,366 frames); every frame then read as it would arrive. The held-out half (37,577 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 54 | 84 | 5 |
| shoulder lift | 54 | 128 | 12 |
| elbow flex | 21 | 144 | 12 |
| wrist flex | 24 | 114 | 8 |
| wrist roll | 25 | 117 | 13 |
| gripper | 24 | 130 | 0 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.314 | 0.491 | 1.128 | 0.832 |
| shoulder lift | 1.266 | 1.646 | 3.113 | 2.321 |
| elbow flex | 0.746 | 1.507 | 3.845 | 2.923 |
| wrist flex | 1.936 | 2.064 | 2.037 | 1.432 |
| wrist roll | 0.233 | 0.250 | 0.702 | 0.486 |
| gripper | 3.663 | 3.785 | 4.730 | 4.133 |
| five body joints | 1.100 | 1.381 | 2.465 | 1.838 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
