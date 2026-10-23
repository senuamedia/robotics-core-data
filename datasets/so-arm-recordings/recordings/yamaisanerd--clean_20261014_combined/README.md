# yamaisanerd/clean_20261014_combined

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [yamaisanerd/clean_20261014_combined](https://huggingface.co/datasets/yamaisanerd/clean_20261014_combined), revision `d1e44058181ebfd9646596f8af3b71e798879075` |
| Author | Nguyen Duy Son ([yamaisanerd](https://huggingface.co/yamaisanerd)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 310 episodes, 196,683 frames |
| Rank | 81 of the selection (one dataset an author, the largest by frames) |

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

Each joint's machine built once from the first half of the episodes (95,263 frames); every frame then read as it would arrive. The held-out half (101,420 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 61 | 80 | 8 |
| shoulder lift | 56 | 113 | 78 |
| elbow flex | 60 | 115 | 44 |
| wrist flex | 71 | 86 | 32 |
| wrist roll | 55 | 70 | 33 |
| gripper | 28 | 128 | 45 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.444 | 0.341 | 1.037 | 0.654 |
| shoulder lift | 2.397 | 2.996 | 5.770 | 5.040 |
| elbow flex | 2.783 | 3.613 | 6.169 | 5.385 |
| wrist flex | 3.313 | 3.285 | 1.183 | 0.853 |
| wrist roll | 0.392 | 0.277 | 0.895 | 0.591 |
| gripper | 9.526 | 9.560 | 11.665 | 11.026 |
| five body joints | 2.228 | 2.569 | 3.862 | 3.344 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
