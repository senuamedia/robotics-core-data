# sabinMlminator/so101_pickplace_cubes_test1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [sabinMlminator/so101_pickplace_cubes_test1](https://huggingface.co/datasets/sabinMlminator/so101_pickplace_cubes_test1), revision `8eacc9be761b90099f3e762bf2384366e4690563` |
| Author | Sabin Grube Doiz ([sabinMlminator](https://huggingface.co/sabinMlminator)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 202 episodes, 208,133 frames |
| Rank | 72 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07383 of LeRobot's calibrated scale (a range of 2709 counts) | 2174 | 2224 |
| shoulder lift | 0.08525 of LeRobot's calibrated scale (a range of 2346 counts) | 2277 | 2294 |
| elbow flex | 0.09058 of LeRobot's calibrated scale (a range of 2208 counts) | 2196 | 2211 |
| wrist flex | 0.08909 of LeRobot's calibrated scale (a range of 2245 counts) | 1272 | 1272 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1477 | 1378 |
| gripper | 0.06983 of LeRobot's calibrated scale (a range of 1432 counts) | 748 | 632 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (111,493 frames); every frame then read as it would arrive. The held-out half (96,640 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 48 | 225 | 42 |
| shoulder lift | 78 | 256 | 49 |
| elbow flex | 44 | 289 | 40 |
| wrist flex | 58 | 268 | 50 |
| wrist roll | 42 | 206 | 22 |
| gripper | 14 | 249 | 37 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.787 | 1.174 | 2.329 | 1.715 |
| shoulder lift | 1.291 | 1.952 | 3.896 | 2.899 |
| elbow flex | 1.632 | 2.290 | 4.436 | 3.556 |
| wrist flex | 1.801 | 2.701 | 4.713 | 3.470 |
| wrist roll | 0.518 | 0.743 | 1.665 | 1.190 |
| gripper | 10.416 | 10.400 | 11.198 | 10.897 |
| five body joints | 1.300 | 1.911 | 3.613 | 2.736 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
