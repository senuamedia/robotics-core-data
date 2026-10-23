# lerobot-projects/2_bin_recycle_merged_20261022

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [lerobot-projects/2_bin_recycle_merged_20261022](https://huggingface.co/datasets/lerobot-projects/2_bin_recycle_merged_20261022), revision `15ea77ab98d609fce9b7c455bd5b7b3f5b967795` |
| Author | LeRobot Projects ([lerobot-projects](https://huggingface.co/lerobot-projects)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 198 episodes, 148,386 frames |
| Rank | 111 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 875 | 894 |
| shoulder lift | 360/4,095 degree (degrees) | 1667 | 1669 |
| elbow flex | 360/4,095 degree (degrees) | 1717 | 1754 |
| wrist flex | 360/4,095 degree (degrees) | 1033 | 1079 |
| wrist roll | 360/4,095 degree (degrees) | 1195 | 1217 |
| gripper | 0.06605 of LeRobot's calibrated scale (a range of 1514 counts) | 581 | 499 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (74,291 frames); every frame then read as it would arrive. The held-out half (74,095 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 55 | 101 | 1 |
| shoulder lift | 59 | 151 | 1 |
| elbow flex | 72 | 158 | 1 |
| wrist flex | 67 | 127 | 3 |
| wrist roll | 35 | 112 | 8 |
| gripper | 32 | 116 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.290 | 0.329 | 0.783 | 0.579 |
| shoulder lift | 1.535 | 1.752 | 2.307 | 1.741 |
| elbow flex | 1.204 | 1.394 | 2.942 | 2.575 |
| wrist flex | 0.349 | 0.363 | 1.161 | 0.798 |
| wrist roll | 0.261 | 0.395 | 1.015 | 0.699 |
| gripper | 2.637 | 2.649 | 2.813 | 2.700 |
| five body joints | 0.903 | 1.039 | 1.842 | 1.491 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
