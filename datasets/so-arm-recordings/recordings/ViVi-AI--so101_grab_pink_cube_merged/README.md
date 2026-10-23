# ViVi-AI/so101_grab_pink_cube_merged

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [ViVi-AI/so101_grab_pink_cube_merged](https://huggingface.co/datasets/ViVi-AI/so101_grab_pink_cube_merged), revision `b95afa84ba95823969e413ad81bacf3e43b1a1a1` |
| Author | [ViVi-AI](https://huggingface.co/ViVi-AI) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 140 episodes, 111,154 frames |
| Rank | 172 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1106 | 1118 |
| shoulder lift | 360/4,095 degree (degrees) | 1982 | 1996 |
| elbow flex | 360/4,095 degree (degrees) | 2048 | 2092 |
| wrist flex | 360/4,095 degree (degrees) | 1067 | 1081 |
| wrist roll | 360/4,095 degree (degrees) | 1131 | 1139 |
| gripper | 0.06743 of LeRobot's calibrated scale (a range of 1483 counts) | 850 | 728 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (56,630 frames); every frame then read as it would arrive. The held-out half (54,524 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 52 | 68 | 112 |
| shoulder lift | 56 | 112 | 3 |
| elbow flex | 57 | 114 | 23 |
| wrist flex | 37 | 98 | 0 |
| wrist roll | 23 | 85 | 0 |
| gripper | 12 | 121 | 7 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.355 | 0.675 | 1.720 | 1.116 |
| shoulder lift | 1.638 | 1.907 | 3.025 | 2.213 |
| elbow flex | 0.859 | 1.257 | 3.237 | 2.494 |
| wrist flex | 1.644 | 1.661 | 1.194 | 0.792 |
| wrist roll | 0.331 | 0.616 | 1.597 | 1.050 |
| gripper | 5.799 | 5.808 | 6.088 | 5.954 |
| five body joints | 1.127 | 1.327 | 2.305 | 1.678 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
