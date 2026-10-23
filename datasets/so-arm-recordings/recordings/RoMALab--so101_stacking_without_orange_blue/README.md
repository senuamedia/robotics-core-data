# RoMALab/so101_stacking_without_orange_blue

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [RoMALab/so101_stacking_without_orange_blue](https://huggingface.co/datasets/RoMALab/so101_stacking_without_orange_blue), revision `bcdc3bbb0500f94c9b67529ae57510267d1efa3d` |
| Author | Robotics & Multimodal Autonomy (RoMA) Lab ([RoMALab](https://huggingface.co/RoMALab)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 251 episodes, 142,009 frames |
| Rank | 121 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.10433 of LeRobot's calibrated scale (a range of 1917 counts) | 1063 | 1501 |
| shoulder lift | 0.08358 of LeRobot's calibrated scale (a range of 2393 counts) | 1873 | 1866 |
| elbow flex | 0.09005 of LeRobot's calibrated scale (a range of 2221 counts) | 1789 | 1852 |
| wrist flex | 0.08496 of LeRobot's calibrated scale (a range of 2354 counts) | 1066 | 1090 |
| wrist roll | 0.05242 of LeRobot's calibrated scale (a range of 3815 counts) | 1084 | 1124 |
| gripper | 0.06662 of LeRobot's calibrated scale (a range of 1501 counts) | 659 | 580 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (69,920 frames); every frame then read as it would arrive. The held-out half (72,089 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 52 | 68 | 4 |
| shoulder lift | 48 | 112 | 16 |
| elbow flex | 53 | 112 | 10 |
| wrist flex | 42 | 98 | 5 |
| wrist roll | 42 | 99 | 1 |
| gripper | 24 | 112 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.230 | 0.396 | 1.150 | 0.772 |
| shoulder lift | 1.540 | 1.800 | 2.702 | 2.099 |
| elbow flex | 0.960 | 1.218 | 2.870 | 2.337 |
| wrist flex | 0.606 | 0.692 | 1.186 | 0.874 |
| wrist roll | 0.427 | 0.552 | 1.384 | 0.890 |
| gripper | 9.920 | 9.928 | 9.827 | 9.762 |
| five body joints | 0.882 | 1.064 | 2.009 | 1.550 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
