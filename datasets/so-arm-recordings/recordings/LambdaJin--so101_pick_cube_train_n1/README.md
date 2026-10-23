# LambdaJin/so101_pick_cube_train_n1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [LambdaJin/so101_pick_cube_train_n1](https://huggingface.co/datasets/LambdaJin/so101_pick_cube_train_n1), revision `74f73688c34860434905d30242394748409f4da9` |
| Author | [LambdaJin](https://huggingface.co/LambdaJin) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 91 episodes, 100,479 frames |
| Rank | 200 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08489 of LeRobot's calibrated scale (a range of 2356 counts) | 886 | 908 |
| shoulder lift | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1691 | 1704 |
| elbow flex | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1542 | 1592 |
| wrist flex | 0.10379 of LeRobot's calibrated scale (a range of 1927 counts) | 720 | 879 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 786 | 803 |
| gripper | 0.06901 of LeRobot's calibrated scale (a range of 1449 counts) | 935 | 811 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,592 frames); every frame then read as it would arrive. The held-out half (55,887 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 15 | 53 | 70 |
| shoulder lift | 39 | 103 | 47 |
| elbow flex | 36 | 86 | 51 |
| wrist flex | 38 | 56 | 24 |
| wrist roll | 16 | 49 | 87 |
| gripper | 10 | 122 | 83 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 61.1 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.486 | 0.277 | 0.848 | 0.562 |
| shoulder lift | 16.047 | 16.135 | 2.817 | 2.258 |
| elbow flex | 1.609 | 1.741 | 2.809 | 2.382 |
| wrist flex | 0.314 | 0.307 | 0.635 | 0.458 |
| wrist roll | 0.477 | 0.482 | 1.039 | 0.812 |
| gripper | 10.577 | 10.597 | 10.767 | 10.630 |
| five body joints | 7.220 | 7.263 | 1.899 | 1.546 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
