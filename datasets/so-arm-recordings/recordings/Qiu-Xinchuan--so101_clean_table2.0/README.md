# Qiu-Xinchuan/so101_clean_table2.0

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Qiu-Xinchuan/so101_clean_table2.0](https://huggingface.co/datasets/Qiu-Xinchuan/so101_clean_table2.0), revision `360f03f1282289097fc9b8f3baeae2d866ff6d14` |
| Author | qxc ([Qiu-Xinchuan](https://huggingface.co/Qiu-Xinchuan)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 205,671 frames |
| Rank | 76 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07257 of LeRobot's calibrated scale (a range of 2756 counts) | 1793 | 1998 |
| shoulder lift | 0.08344 of LeRobot's calibrated scale (a range of 2397 counts) | 2358 | 2368 |
| elbow flex | 0.08997 of LeRobot's calibrated scale (a range of 2223 counts) | 2202 | 2195 |
| wrist flex | 0.08410 of LeRobot's calibrated scale (a range of 2378 counts) | 1616 | 1607 |
| wrist roll | 0.05212 of LeRobot's calibrated scale (a range of 3837 counts) | 2036 | 2050 |
| gripper | 0.06297 of LeRobot's calibrated scale (a range of 1588 counts) | 1078 | 919 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (89,121 frames); every frame then read as it would arrive. The held-out half (116,550 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 84 | 131 | 8 |
| shoulder lift | 51 | 155 | 2 |
| elbow flex | 45 | 172 | 2 |
| wrist flex | 57 | 168 | 157 |
| wrist roll | 28 | 150 | 59 |
| gripper | 27 | 126 | 4 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.843 | 2.527 | 4.572 | 3.309 |
| shoulder lift | 2.321 | 3.223 | 4.042 | 2.954 |
| elbow flex | 1.558 | 2.583 | 4.749 | 3.585 |
| wrist flex | 3.230 | 3.869 | 4.651 | 3.332 |
| wrist roll | 0.559 | 2.375 | 4.379 | 3.177 |
| gripper | 4.042 | 4.876 | 8.680 | 7.345 |
| five body joints | 1.963 | 2.968 | 4.485 | 3.278 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
