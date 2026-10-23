# MinhDuk/clean_20261014_full

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [MinhDuk/clean_20261014_full](https://huggingface.co/datasets/MinhDuk/clean_20261014_full), revision `c85535e92a6f37bdf6356675c84533cf5503cd91` |
| Author | nguyen minh duc ([MinhDuk](https://huggingface.co/MinhDuk)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 310 episodes, 196,683 frames |
| Rank | 80 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1122 | 1148 |
| shoulder lift | 360/4,095 degree (degrees) | 2126 | 2162 |
| elbow flex | 360/4,095 degree (degrees) | 2187 | 2247 |
| wrist flex | 360/4,095 degree (degrees) | 1047 | 1061 |
| wrist roll | 360/4,095 degree (degrees) | 1252 | 1286 |
| gripper | 0.06313 of LeRobot's calibrated scale (a range of 1584 counts) | 1325 | 1188 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (101,179 frames); every frame then read as it would arrive. The held-out half (95,504 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 59 | 87 | 3 |
| shoulder lift | 80 | 145 | 6 |
| elbow flex | 63 | 132 | 5 |
| wrist flex | 52 | 107 | 2 |
| wrist roll | 37 | 93 | 0 |
| gripper | 29 | 131 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.460 | 0.352 | 1.169 | 0.701 |
| shoulder lift | 1.981 | 2.422 | 4.792 | 4.118 |
| elbow flex | 2.857 | 3.309 | 5.527 | 5.049 |
| wrist flex | 2.779 | 2.776 | 1.260 | 0.941 |
| wrist roll | 0.427 | 0.247 | 0.943 | 0.561 |
| gripper | 8.796 | 8.869 | 11.098 | 10.380 |
| five body joints | 2.010 | 2.222 | 3.386 | 2.970 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
