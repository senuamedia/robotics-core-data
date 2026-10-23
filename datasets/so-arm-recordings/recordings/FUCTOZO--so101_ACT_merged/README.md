# FUCTOZO/so101_ACT_merged

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [FUCTOZO/so101_ACT_merged](https://huggingface.co/datasets/FUCTOZO/so101_ACT_merged), revision `6ba0b249651b0d262c7fc3c9552ab717e864b36e` |
| Author | [FUCTOZO](https://huggingface.co/FUCTOZO) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 190 episodes, 92,273 frames |
| Rank | 220 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1292 | 1310 |
| shoulder lift | 360/4,095 degree (degrees) | 1859 | 1864 |
| elbow flex | 360/4,095 degree (degrees) | 1776 | 1817 |
| wrist flex | 360/4,095 degree (degrees) | 848 | 869 |
| wrist roll | 360/4,095 degree (degrees) | 1276 | 1285 |
| gripper | 0.06897 of LeRobot's calibrated scale (a range of 1450 counts) | 812 | 689 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (43,851 frames); every frame then read as it would arrive. The held-out half (48,422 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 35 | 147 | 0 |
| shoulder lift | 42 | 164 | 8 |
| elbow flex | 31 | 182 | 29 |
| wrist flex | 44 | 131 | 0 |
| wrist roll | 32 | 90 | 183 |
| gripper | 32 | 157 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.450 | 0.739 | 1.894 | 1.232 |
| shoulder lift | 1.261 | 1.592 | 2.902 | 2.070 |
| elbow flex | 0.769 | 1.401 | 3.438 | 2.695 |
| wrist flex | 2.815 | 2.843 | 1.388 | 0.946 |
| wrist roll | 0.413 | 0.426 | 1.063 | 0.696 |
| gripper | 4.594 | 4.642 | 6.115 | 5.503 |
| five body joints | 1.447 | 1.631 | 2.318 | 1.699 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
