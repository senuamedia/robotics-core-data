# bfpl/so101_green_lego_block_into_bowl

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [bfpl/so101_green_lego_block_into_bowl](https://huggingface.co/datasets/bfpl/so101_green_lego_block_into_bowl), revision `6ffcb97d7c551b45864821e8cca01a4e0ebb3f5d` |
| Author | [bfpl](https://huggingface.co/bfpl) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 119 episodes, 106,098 frames |
| Rank | 179 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1092 | 1098 |
| shoulder lift | 360/4,095 degree (degrees) | 2095 | 2070 |
| elbow flex | 360/4,095 degree (degrees) | 2190 | 2206 |
| wrist flex | 360/4,095 degree (degrees) | 1774 | 1801 |
| wrist roll | 360/4,095 degree (degrees) | 1178 | 1187 |
| gripper | 0.06258 of LeRobot's calibrated scale (a range of 1598 counts) | 913 | 831 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (46,215 frames); every frame then read as it would arrive. The held-out half (59,883 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 34 | 41 | 12 |
| shoulder lift | 43 | 51 | 50 |
| elbow flex | 42 | 47 | 0 |
| wrist flex | 40 | 46 | 5 |
| wrist roll | 25 | 39 | 5 |
| gripper | 18 | 57 | 5 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.189 | 0.305 | 1.045 | 0.682 |
| shoulder lift | 16.816 | 17.415 | 18.476 | 17.763 |
| elbow flex | 21.001 | 21.448 | 23.242 | 22.672 |
| wrist flex | 2.620 | 2.635 | 1.195 | 0.878 |
| wrist roll | 0.253 | 0.237 | 0.546 | 0.418 |
| gripper | 1.627 | 1.710 | 2.151 | 1.816 |
| five body joints | 12.089 | 12.412 | 13.299 | 12.890 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
