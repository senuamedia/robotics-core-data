# nevertmr/so101_coffee_subtask

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [nevertmr/so101_coffee_subtask](https://huggingface.co/datasets/nevertmr/so101_coffee_subtask), revision `d00d9c1366d804529d992581145aeecf1c1c5e59` |
| Author | [nevertmr](https://huggingface.co/nevertmr) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 431 episodes, 99,472 frames |
| Rank | 202 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.10070 of LeRobot's calibrated scale (a range of 1986 counts) | 1572 | 1590 |
| shoulder lift | 0.08365 of LeRobot's calibrated scale (a range of 2391 counts) | 1901 | 1915 |
| elbow flex | 0.08965 of LeRobot's calibrated scale (a range of 2231 counts) | 1393 | 1465 |
| wrist flex | 0.08507 of LeRobot's calibrated scale (a range of 2351 counts) | 1392 | 1422 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 235 | 249 |
| gripper | 0.09242 of LeRobot's calibrated scale (a range of 1082 counts) | 680 | 810 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (51,673 frames); every frame then read as it would arrive. The held-out half (47,799 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 39 | 255 | 60 |
| shoulder lift | 59 | 295 | 54 |
| elbow flex | 42 | 290 | 0 |
| wrist flex | 29 | 256 | 58 |
| wrist roll | 15 | 85 | 6 |
| gripper | 21 | 227 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.633 | 0.730 | 1.261 | 1.015 |
| shoulder lift | 2.252 | 2.424 | 3.879 | 2.644 |
| elbow flex | 0.814 | 1.852 | 4.165 | 2.979 |
| wrist flex | 0.556 | 1.251 | 2.750 | 1.780 |
| wrist roll | 0.191 | 0.168 | 0.413 | 0.374 |
| gripper | 0.750 | 1.139 | 2.208 | 1.563 |
| five body joints | 1.138 | 1.512 | 2.888 | 2.010 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
