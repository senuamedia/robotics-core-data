# Jinny9220/so101_test_v1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Jinny9220/so101_test_v1](https://huggingface.co/datasets/Jinny9220/so101_test_v1), revision `2453ba021cd8dc0155af679ee7b9d6905193d260` |
| Author | [Jinny9220](https://huggingface.co/Jinny9220) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 50 episodes, 88,598 frames |
| Rank | 239 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08344 of LeRobot's calibrated scale (a range of 2397 counts) | 1152 | 1296 |
| shoulder lift | 0.10225 of LeRobot's calibrated scale (a range of 1956 counts) | 1209 | 1133 |
| elbow flex | 0.11357 of LeRobot's calibrated scale (a range of 1761 counts) | 1255 | 1319 |
| wrist flex | 0.09995 of LeRobot's calibrated scale (a range of 2001 counts) | 704 | 728 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 104 | 89 |
| gripper | 0.06906 of LeRobot's calibrated scale (a range of 1448 counts) | 1062 | 961 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,298 frames); every frame then read as it would arrive. The held-out half (44,300 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 38 | 82 | 1 |
| shoulder lift | 42 | 104 | 7 |
| elbow flex | 23 | 87 | 0 |
| wrist flex | 19 | 84 | 0 |
| wrist roll | 30 | 31 | 11 |
| gripper | 13 | 124 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 61.1 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.335 | 0.964 | 1.924 | 1.377 |
| shoulder lift | 1.400 | 1.804 | 2.662 | 2.160 |
| elbow flex | 1.304 | 1.614 | 3.306 | 2.940 |
| wrist flex | 0.413 | 0.708 | 1.372 | 1.015 |
| wrist roll | 0.185 | 0.186 | 0.270 | 0.268 |
| gripper | 7.332 | 7.668 | 9.255 | 8.559 |
| five body joints | 0.892 | 1.210 | 2.176 | 1.806 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
