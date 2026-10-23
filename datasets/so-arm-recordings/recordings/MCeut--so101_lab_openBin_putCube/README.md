# MCeut/so101_lab_openBin_putCube

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [MCeut/so101_lab_openBin_putCube](https://huggingface.co/datasets/MCeut/so101_lab_openBin_putCube), revision `ac5456f3177cdf6d07de9b9539092b230aa3ecd4` |
| Author | [MCeut](https://huggingface.co/MCeut) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 103 episodes, 104,413 frames |
| Rank | 189 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07353 of LeRobot's calibrated scale (a range of 2720 counts) | 2300 | 2328 |
| shoulder lift | 0.08514 of LeRobot's calibrated scale (a range of 2349 counts) | 1937 | 1874 |
| elbow flex | 0.09062 of LeRobot's calibrated scale (a range of 2207 counts) | 1752 | 1812 |
| wrist flex | 0.08881 of LeRobot's calibrated scale (a range of 2252 counts) | 1964 | 2016 |
| wrist roll | 0.05145 of LeRobot's calibrated scale (a range of 3887 counts) | 1276 | 1253 |
| gripper | 0.07022 of LeRobot's calibrated scale (a range of 1424 counts) | 730 | 627 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (50,067 frames); every frame then read as it would arrive. The held-out half (54,346 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 38 | 159 | 12 |
| shoulder lift | 67 | 168 | 11 |
| elbow flex | 65 | 182 | 18 |
| wrist flex | 69 | 180 | 19 |
| wrist roll | 61 | 139 | 15 |
| gripper | 15 | 185 | 41 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.783 | 1.060 | 1.955 | 1.469 |
| shoulder lift | 1.088 | 1.524 | 2.844 | 2.100 |
| elbow flex | 1.411 | 1.770 | 3.231 | 2.730 |
| wrist flex | 0.588 | 1.077 | 2.218 | 1.553 |
| wrist roll | 0.344 | 0.507 | 1.095 | 0.787 |
| gripper | 0.403 | 0.581 | 1.182 | 0.789 |
| five body joints | 0.921 | 1.264 | 2.386 | 1.846 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
