# subhodipsaha/so101_stack_three_cubes_08_19

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [subhodipsaha/so101_stack_three_cubes_08_19](https://huggingface.co/datasets/subhodipsaha/so101_stack_three_cubes_08_19), revision `8311b76246ef0a02f33f996edbef40031251a85b` |
| Author | [subhodipsaha](https://huggingface.co/subhodipsaha) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 25 frames a second |
| Recording | 119 episodes, 74,399 frames |
| Rank | 295 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 755 | 761 |
| shoulder lift | 360/4,095 degree (degrees) | 1526 | 1552 |
| elbow flex | 360/4,095 degree (degrees) | 1218 | 1249 |
| wrist flex | 360/4,095 degree (degrees) | 808 | 829 |
| wrist roll | 360/4,095 degree (degrees) | 1042 | 1062 |
| gripper | 0.07072 of LeRobot's calibrated scale (a range of 1414 counts) | 743 | 771 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,105 frames); every frame then read as it would arrive. The held-out half (30,294 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 51 | 60 | 0 |
| shoulder lift | 35 | 111 | 69 |
| elbow flex | 45 | 87 | 64 |
| wrist flex | 25 | 114 | 7 |
| wrist roll | 82 | 84 | 51 |
| gripper | 25 | 145 | 2 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 499 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 34.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.321 | 0.275 | 0.972 | 0.507 |
| shoulder lift | 1.560 | 1.920 | 3.040 | 2.060 |
| elbow flex | 0.570 | 0.870 | 2.723 | 2.234 |
| wrist flex | 0.412 | 0.555 | 1.294 | 0.842 |
| wrist roll | 0.162 | 0.139 | 0.277 | 0.181 |
| gripper | 6.671 | 6.794 | 7.292 | 6.780 |
| five body joints | 0.781 | 0.984 | 1.966 | 1.429 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
