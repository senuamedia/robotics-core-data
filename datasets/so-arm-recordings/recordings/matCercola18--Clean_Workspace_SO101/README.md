# matCercola18/Clean_Workspace_SO101

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [matCercola18/Clean_Workspace_SO101](https://huggingface.co/datasets/matCercola18/Clean_Workspace_SO101), revision `b1a431b12a4930b9ac233ceff28bab7710c18d94` |
| Author | Matteo Cercola ([matCercola18](https://huggingface.co/matCercola18)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 240 episodes, 145,794 frames |
| Rank | 114 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2226 | 2344 |
| shoulder lift | 360/4,095 degree (degrees) | 2117 | 2111 |
| elbow flex | 360/4,095 degree (degrees) | 2061 | 2109 |
| wrist flex | 360/4,095 degree (degrees) | 1460 | 1496 |
| wrist roll | 360/4,095 degree (degrees) | 1521 | 1512 |
| gripper | 0.06748 of LeRobot's calibrated scale (a range of 1482 counts) | 1239 | 1043 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (66,607 frames); every frame then read as it would arrive. The held-out half (79,187 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 53 | 138 | 0 |
| shoulder lift | 60 | 140 | 0 |
| elbow flex | 50 | 139 | 8 |
| wrist flex | 99 | 132 | 1 |
| wrist roll | 36 | 111 | 21 |
| gripper | 14 | 137 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.702 | 1.101 | 2.242 | 1.556 |
| shoulder lift | 1.153 | 1.291 | 2.224 | 1.464 |
| elbow flex | 1.121 | 1.419 | 3.015 | 2.377 |
| wrist flex | 0.556 | 0.653 | 1.638 | 1.107 |
| wrist roll | 0.363 | 0.328 | 0.760 | 0.538 |
| gripper | 0.051 | 0.051 | 0.253 | 0.252 |
| five body joints | 0.838 | 1.041 | 2.112 | 1.531 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
