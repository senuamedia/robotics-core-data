# AkshayM-X/so101_pick_place_merged_first_400Epi

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [AkshayM-X/so101_pick_place_merged_first_400Epi](https://huggingface.co/datasets/AkshayM-X/so101_pick_place_merged_first_400Epi), revision `b8286b2151253b4632405ecdeb13ee0e2e32d996` |
| Author | Akshay Ganesh Mhetre ([AkshayM-X](https://huggingface.co/AkshayM-X)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 400 episodes, 448,726 frames |
| Rank | 28 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1895 | 1917 |
| shoulder lift | 360/4,095 degree (degrees) | 1913 | 1934 |
| elbow flex | 360/4,095 degree (degrees) | 1998 | 2159 |
| wrist flex | 360/4,095 degree (degrees) | 989 | 1000 |
| wrist roll | 360/4,095 degree (degrees) | 2504 | 2525 |
| gripper | 0.06906 of LeRobot's calibrated scale (a range of 1448 counts) | 838 | 694 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (269,217 frames); every frame then read as it would arrive. The held-out half (179,509 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 75 | 107 | 21 |
| shoulder lift | 71 | 138 | 15 |
| elbow flex | 84 | 190 | 5 |
| wrist flex | 68 | 99 | 2 |
| wrist roll | 55 | 129 | 2 |
| gripper | 11 | 118 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.507 | 0.967 | 2.119 | 1.408 |
| shoulder lift | 10.526 | 10.596 | 3.669 | 2.576 |
| elbow flex | 1.962 | 2.285 | 3.227 | 2.463 |
| wrist flex | 0.520 | 0.606 | 1.345 | 0.936 |
| wrist roll | 0.306 | 0.383 | 1.009 | 0.714 |
| gripper | 8.862 | 8.863 | 8.945 | 8.816 |
| five body joints | 4.801 | 4.877 | 2.497 | 1.792 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
