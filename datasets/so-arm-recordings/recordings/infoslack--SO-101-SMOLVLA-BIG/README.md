# infoslack/SO-101-SMOLVLA-BIG

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [infoslack/SO-101-SMOLVLA-BIG](https://huggingface.co/datasets/infoslack/SO-101-SMOLVLA-BIG), revision `76e052c4735aa17a780ea0cf33df1760700276ee` |
| Author | Daniel Romero ([infoslack](https://huggingface.co/infoslack)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 250 episodes, 195,461 frames |
| Rank | 83 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08453 of LeRobot's calibrated scale (a range of 2366 counts) | 931 | 973 |
| shoulder lift | 0.08737 of LeRobot's calibrated scale (a range of 2289 counts) | 2074 | 2176 |
| elbow flex | 0.09174 of LeRobot's calibrated scale (a range of 2180 counts) | 1962 | 2028 |
| wrist flex | 0.08639 of LeRobot's calibrated scale (a range of 2315 counts) | 1786 | 1850 |
| wrist roll | 0.05255 of LeRobot's calibrated scale (a range of 3806 counts) | 1695 | 1711 |
| gripper | 0.07067 of LeRobot's calibrated scale (a range of 1415 counts) | 595 | 592 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (88,463 frames); every frame then read as it would arrive. The held-out half (106,998 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 64 | 85 | 2 |
| shoulder lift | 52 | 109 | 102 |
| elbow flex | 48 | 134 | 84 |
| wrist flex | 61 | 130 | 25 |
| wrist roll | 30 | 112 | 79 |
| gripper | 10 | 118 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.994 | 1.079 | 1.350 | 1.232 |
| shoulder lift | 1.800 | 2.046 | 2.438 | 1.965 |
| elbow flex | 1.175 | 1.489 | 3.185 | 2.774 |
| wrist flex | 0.526 | 0.592 | 1.031 | 0.897 |
| wrist roll | 0.304 | 0.295 | 0.690 | 0.548 |
| gripper | 7.866 | 7.903 | 7.921 | 7.831 |
| five body joints | 1.093 | 1.265 | 1.972 | 1.683 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
