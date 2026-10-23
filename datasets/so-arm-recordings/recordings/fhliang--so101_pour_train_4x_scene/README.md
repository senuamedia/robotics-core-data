# fhliang/so101_pour_train_4x_scene

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [fhliang/so101_pour_train_4x_scene](https://huggingface.co/datasets/fhliang/so101_pour_train_4x_scene), revision `a2a9ace099f957568eb9ddd7054c5ec9dac82639` |
| Author | Freddie Liang ([fhliang](https://huggingface.co/fhliang)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 225 episodes, 148,530 frames |
| Rank | 110 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 661 | 851 |
| shoulder lift | 360/4,095 degree (degrees) | 2084 | 2121 |
| elbow flex | 360/4,095 degree (degrees) | 1878 | 1919 |
| wrist flex | 360/4,095 degree (degrees) | 2001 | 2017 |
| wrist roll | 360/4,095 degree (degrees) | 2949 | 3606 |
| gripper | 0.06566 of LeRobot's calibrated scale (a range of 1523 counts) | 767 | 679 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (77,489 frames); every frame then read as it would arrive. The held-out half (71,041 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 48 | 128 | 0 |
| shoulder lift | 39 | 172 | 0 |
| elbow flex | 55 | 178 | 0 |
| wrist flex | 67 | 196 | 0 |
| wrist roll | 62 | 225 | 0 |
| gripper | 30 | 160 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.889 | 1.074 | 1.534 | 1.294 |
| shoulder lift | 3.070 | 3.318 | 4.637 | 4.378 |
| elbow flex | 2.495 | 2.833 | 5.204 | 4.887 |
| wrist flex | 1.371 | 1.568 | 2.841 | 2.438 |
| wrist roll | 33.168 | 33.369 | 33.803 | 33.554 |
| gripper | 1.937 | 1.968 | 2.087 | 1.901 |
| five body joints | 14.955 | 15.074 | 15.502 | 15.339 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
