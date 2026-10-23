# Niels2/so101-demonstrations-robotcam-640p-30fps

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Niels2/so101-demonstrations-robotcam-640p-30fps](https://huggingface.co/datasets/Niels2/so101-demonstrations-robotcam-640p-30fps), revision `d6d59e233c4d99d198d85f42bf22478b703aeb9f` |
| Author | [Niels2](https://huggingface.co/Niels2) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 331 episodes, 88,883 frames |
| Rank | 237 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08450 of LeRobot's calibrated scale (a range of 2367 counts) | 1617 | 1866 |
| shoulder lift | 0.08295 of LeRobot's calibrated scale (a range of 2411 counts) | 1906 | 1863 |
| elbow flex | 0.09009 of LeRobot's calibrated scale (a range of 2220 counts) | 1747 | 1755 |
| wrist flex | 0.08511 of LeRobot's calibrated scale (a range of 2350 counts) | 1156 | 1216 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1631 | 1402 |
| gripper | 0.06807 of LeRobot's calibrated scale (a range of 1469 counts) | 872 | 680 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (49,258 frames); every frame then read as it would arrive. The held-out half (39,625 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 38 | 136 | 13 |
| shoulder lift | 39 | 128 | 2 |
| elbow flex | 44 | 133 | 1 |
| wrist flex | 54 | 127 | 9 |
| wrist roll | 32 | 166 | 33 |
| gripper | 23 | 228 | 3 |

## The simulator against this arm

A development arm (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.413 | 0.951 | 1.965 | 1.398 |
| shoulder lift | 1.418 | 3.536 | 6.133 | 4.655 |
| elbow flex | 1.727 | 3.495 | 6.553 | 5.294 |
| wrist flex | 0.981 | 1.766 | 3.178 | 2.328 |
| wrist roll | 0.758 | 1.912 | 3.667 | 2.585 |
| gripper | 15.901 | 16.373 | 18.105 | 17.523 |
| five body joints | 1.157 | 2.545 | 4.646 | 3.570 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
