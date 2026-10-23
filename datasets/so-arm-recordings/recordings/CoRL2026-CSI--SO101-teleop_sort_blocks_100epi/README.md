# CoRL2026-CSI/SO101-teleop_sort_blocks_100epi

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [CoRL2026-CSI/SO101-teleop_sort_blocks_100epi](https://huggingface.co/datasets/CoRL2026-CSI/SO101-teleop_sort_blocks_100epi), revision `7a14a11506adbb03070b7bb20316dfdd914afaf1` |
| Author | [CoRL2026-CSI](https://huggingface.co/CoRL2026-CSI) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 94,568 frames |
| Rank | 213 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07952 of LeRobot's calibrated scale (a range of 2515 counts) | 1692 | 1718 |
| shoulder lift | 0.08540 of LeRobot's calibrated scale (a range of 2342 counts) | 2060 | 2128 |
| elbow flex | 0.09021 of LeRobot's calibrated scale (a range of 2217 counts) | 2166 | 2237 |
| wrist flex | 0.08558 of LeRobot's calibrated scale (a range of 2337 counts) | 877 | 899 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 653 | 685 |
| gripper | 0.06494 of LeRobot's calibrated scale (a range of 1540 counts) | 1052 | 912 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (47,239 frames); every frame then read as it would arrive. The held-out half (47,329 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 60 | 73 | 2 |
| shoulder lift | 56 | 91 | 1 |
| elbow flex | 58 | 96 | 2 |
| wrist flex | 27 | 86 | 1 |
| wrist roll | 23 | 74 | 3 |
| gripper | 17 | 111 | 6 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 61.1 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.605 | 0.929 | 1.937 | 1.359 |
| shoulder lift | 1.977 | 2.395 | 3.958 | 2.986 |
| elbow flex | 1.103 | 1.377 | 4.064 | 3.360 |
| wrist flex | 0.742 | 0.740 | 1.546 | 1.190 |
| wrist roll | 0.595 | 0.468 | 0.926 | 0.650 |
| gripper | 6.715 | 7.213 | 9.479 | 8.449 |
| five body joints | 1.131 | 1.361 | 2.799 | 2.185 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
