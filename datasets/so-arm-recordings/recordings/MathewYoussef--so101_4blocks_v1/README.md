# MathewYoussef/so101_4blocks_v1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [MathewYoussef/so101_4blocks_v1](https://huggingface.co/datasets/MathewYoussef/so101_4blocks_v1), revision `b1a9210eeaf528ba04b440bffe51ef091511ddd8` |
| Author | Mathew Youssef ([MathewYoussef](https://huggingface.co/MathewYoussef)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 242 episodes, 241,556 frames |
| Rank | 57 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09770 of LeRobot's calibrated scale (a range of 2047 counts) | 1498 | 1510 |
| shoulder lift | 0.10482 of LeRobot's calibrated scale (a range of 1908 counts) | 1782 | 2052 |
| elbow flex | 0.10086 of LeRobot's calibrated scale (a range of 1983 counts) | 1901 | 2138 |
| wrist flex | 0.09519 of LeRobot's calibrated scale (a range of 2101 counts) | 2099 | 2185 |
| wrist roll | 0.09483 of LeRobot's calibrated scale (a range of 2109 counts) | 2024 | 2187 |
| gripper | 0.09443 of LeRobot's calibrated scale (a range of 1059 counts) | 823 | 709 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (120,299 frames); every frame then read as it would arrive. The held-out half (121,257 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 38 | 113 | 23 |
| shoulder lift | 63 | 141 | 6 |
| elbow flex | 54 | 168 | 55 |
| wrist flex | 51 | 134 | 735 |
| wrist roll | 72 | 145 | 166 |
| gripper | 46 | 125 | 55 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 83.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.680 | 0.346 | 1.035 | 0.735 |
| shoulder lift | 1.470 | 1.470 | 2.334 | 1.769 |
| elbow flex | 1.393 | 1.831 | 3.432 | 2.921 |
| wrist flex | 1.264 | 0.545 | 1.793 | 1.171 |
| wrist roll | 0.248 | 0.108 | 0.370 | 0.315 |
| gripper | 4.269 | 4.187 | 4.282 | 4.201 |
| five body joints | 1.115 | 1.089 | 2.080 | 1.653 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
