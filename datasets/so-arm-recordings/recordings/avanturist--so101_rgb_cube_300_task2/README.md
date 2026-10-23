# avanturist/so101_rgb_cube_300_task2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [avanturist/so101_rgb_cube_300_task2](https://huggingface.co/datasets/avanturist/so101_rgb_cube_300_task2), revision `41ef2926dec224394c2ee28aca1cbddfc903fb1f` |
| Author | [avanturist](https://huggingface.co/avanturist) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 301 episodes, 91,134 frames |
| Rank | 223 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07463 of LeRobot's calibrated scale (a range of 2680 counts) | 1512 | 1554 |
| shoulder lift | 0.08190 of LeRobot's calibrated scale (a range of 2442 counts) | 2164 | 2247 |
| elbow flex | 0.08925 of LeRobot's calibrated scale (a range of 2241 counts) | 1758 | 1819 |
| wrist flex | 0.08114 of LeRobot's calibrated scale (a range of 2465 counts) | 1904 | 1741 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1880 | 1840 |
| gripper | 0.06341 of LeRobot's calibrated scale (a range of 1577 counts) | 1083 | 987 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (45,977 frames); every frame then read as it would arrive. The held-out half (45,157 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 95 | 21 |
| shoulder lift | 42 | 136 | 60 |
| elbow flex | 41 | 138 | 32 |
| wrist flex | 46 | 174 | 86 |
| wrist roll | 41 | 160 | 37 |
| gripper | 20 | 168 | 0 |

## The simulator against this arm

A development arm (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.624 | 1.255 | 2.334 | 1.672 |
| shoulder lift | 1.800 | 2.869 | 4.842 | 3.570 |
| elbow flex | 0.759 | 1.517 | 3.265 | 2.545 |
| wrist flex | 0.976 | 2.322 | 4.304 | 3.009 |
| wrist roll | 0.563 | 0.940 | 1.866 | 1.330 |
| gripper | 17.299 | 17.615 | 19.207 | 18.724 |
| five body joints | 1.046 | 1.917 | 3.508 | 2.562 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
