# soulstarz/so101_pickplace_plus

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [soulstarz/so101_pickplace_plus](https://huggingface.co/datasets/soulstarz/so101_pickplace_plus), revision `09e3853db8e9221da2025e2b8ee03dc0f556aaf2` |
| Author | syq ([soulstarz](https://huggingface.co/soulstarz)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 178,763 frames |
| Rank | 93 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1121 | 1158 |
| shoulder lift | 360/4,095 degree (degrees) | 1899 | 1888 |
| elbow flex | 360/4,095 degree (degrees) | 1930 | 1989 |
| wrist flex | 360/4,095 degree (degrees) | 551 | 633 |
| wrist roll | 360/4,095 degree (degrees) | 1519 | 1556 |
| gripper | 0.06887 of LeRobot's calibrated scale (a range of 1452 counts) | 736 | 615 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (90,591 frames); every frame then read as it would arrive. The held-out half (88,172 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 62 | 81 | 5 |
| shoulder lift | 56 | 77 | 7 |
| elbow flex | 73 | 89 | 1 |
| wrist flex | 50 | 79 | 4 |
| wrist roll | 46 | 105 | 29 |
| gripper | 41 | 106 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.913 | 1.458 | 2.356 | 1.900 |
| shoulder lift | 1.618 | 2.463 | 4.089 | 3.365 |
| elbow flex | 1.507 | 2.147 | 4.478 | 3.909 |
| wrist flex | 0.747 | 0.907 | 1.393 | 1.054 |
| wrist roll | 0.516 | 0.853 | 1.668 | 1.241 |
| gripper | 3.252 | 3.389 | 3.930 | 3.539 |
| five body joints | 1.144 | 1.694 | 3.067 | 2.563 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
