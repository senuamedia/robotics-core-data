# Embodied-AI-6/dataset

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Embodied-AI-6/dataset](https://huggingface.co/datasets/Embodied-AI-6/dataset), revision `3d33e927df43b0e1d198130d61df5e93edc42b6b` |
| Author | [Embodied-AI-6](https://huggingface.co/Embodied-AI-6) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 160 episodes, 81,786 frames |
| Rank | 262 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07342 of LeRobot's calibrated scale (a range of 2724 counts) | 924 | 931 |
| shoulder lift | 0.08811 of LeRobot's calibrated scale (a range of 2270 counts) | 1713 | 1781 |
| elbow flex | 0.08881 of LeRobot's calibrated scale (a range of 2252 counts) | 1609 | 1636 |
| wrist flex | 0.08464 of LeRobot's calibrated scale (a range of 2363 counts) | 1151 | 1198 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1089 | 1073 |
| gripper | 0.06566 of LeRobot's calibrated scale (a range of 1523 counts) | 321 | 301 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (41,889 frames); every frame then read as it would arrive. The held-out half (39,897 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 40 | 56 | 40 |
| shoulder lift | 18 | 122 | 2 |
| elbow flex | 26 | 116 | 12 |
| wrist flex | 32 | 89 | 96 |
| wrist roll | 18 | 113 | 10 |
| gripper | 14 | 101 | 2 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.191 | 0.177 | 0.524 | 0.368 |
| shoulder lift | 1.124 | 1.756 | 3.458 | 2.313 |
| elbow flex | 0.659 | 1.346 | 3.692 | 2.972 |
| wrist flex | 2.135 | 2.618 | 3.637 | 2.976 |
| wrist roll | 0.243 | 0.247 | 0.790 | 0.519 |
| gripper | 2.039 | 2.046 | 2.305 | 2.102 |
| five body joints | 1.126 | 1.538 | 2.818 | 2.165 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
