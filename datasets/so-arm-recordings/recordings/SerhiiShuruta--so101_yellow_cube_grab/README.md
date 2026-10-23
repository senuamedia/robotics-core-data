# SerhiiShuruta/so101_yellow_cube_grab

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [SerhiiShuruta/so101_yellow_cube_grab](https://huggingface.co/datasets/SerhiiShuruta/so101_yellow_cube_grab), revision `d98c1b24d620d7b9afe69b4ea1ffd57886c928f3` |
| Author | Serhii Shuruta ([SerhiiShuruta](https://huggingface.co/SerhiiShuruta)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 201 episodes, 339,481 frames |
| Rank | 38 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.10610 of LeRobot's calibrated scale (a range of 1885 counts) | 1310 | 1311 |
| shoulder lift | 0.08432 of LeRobot's calibrated scale (a range of 2372 counts) | 2022 | 2041 |
| elbow flex | 0.09112 of LeRobot's calibrated scale (a range of 2195 counts) | 1623 | 1668 |
| wrist flex | 0.08551 of LeRobot's calibrated scale (a range of 2339 counts) | 1609 | 1621 |
| wrist roll | 0.05241 of LeRobot's calibrated scale (a range of 3816 counts) | 1535 | 1534 |
| gripper | 0.06835 of LeRobot's calibrated scale (a range of 1463 counts) | 542 | 468 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (178,418 frames); every frame then read as it would arrive. The held-out half (161,063 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 60 | 69 | 64 |
| shoulder lift | 38 | 98 | 377 |
| elbow flex | 46 | 90 | 233 |
| wrist flex | 42 | 85 | 80 |
| wrist roll | 33 | 108 | 217 |
| gripper | 28 | 103 | 97 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.092 | 0.217 | 0.566 | 0.391 |
| shoulder lift | 0.298 | 1.016 | 1.867 | 1.321 |
| elbow flex | 1.954 | 1.892 | 1.437 | 1.244 |
| wrist flex | 0.159 | 0.493 | 1.095 | 0.814 |
| wrist roll | 0.257 | 1.286 | 2.347 | 1.672 |
| gripper | 2.397 | 2.543 | 2.891 | 2.712 |
| five body joints | 0.894 | 1.145 | 1.586 | 1.175 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
