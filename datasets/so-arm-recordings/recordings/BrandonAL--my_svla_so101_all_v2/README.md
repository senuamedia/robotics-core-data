# BrandonAL/my_svla_so101_all_v2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [BrandonAL/my_svla_so101_all_v2](https://huggingface.co/datasets/BrandonAL/my_svla_so101_all_v2), revision `29b13b3d3a4ba91b748c78140b9b93d185aa0d81` |
| Author | [BrandonAL](https://huggingface.co/BrandonAL) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 246 episodes, 104,123 frames |
| Rank | 190 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08389 of LeRobot's calibrated scale (a range of 2384 counts) | 1981 | 2252 |
| shoulder lift | 0.08475 of LeRobot's calibrated scale (a range of 2360 counts) | 2015 | 1998 |
| elbow flex | 0.09025 of LeRobot's calibrated scale (a range of 2216 counts) | 2052 | 2102 |
| wrist flex | 0.08580 of LeRobot's calibrated scale (a range of 2331 counts) | 973 | 983 |
| wrist roll | 0.05280 of LeRobot's calibrated scale (a range of 3788 counts) | 1608 | 1650 |
| gripper | 0.06798 of LeRobot's calibrated scale (a range of 1471 counts) | 587 | 508 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (50,180 frames); every frame then read as it would arrive. The held-out half (53,943 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 21 | 240 | 23 |
| shoulder lift | 121 | 232 | 19 |
| elbow flex | 46 | 258 | 19 |
| wrist flex | 20 | 201 | 73 |
| wrist roll | 64 | 161 | 35 |
| gripper | 6 | 205 | 11 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 77.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 1.461 | 1.218 | 2.823 | 1.865 |
| shoulder lift | 2.022 | 2.878 | 5.656 | 4.397 |
| elbow flex | 3.431 | 5.231 | 8.195 | 6.694 |
| wrist flex | 3.238 | 3.268 | 4.819 | 3.415 |
| wrist roll | 1.529 | 1.335 | 2.998 | 2.027 |
| gripper | 6.129 | 5.791 | 6.460 | 6.000 |
| five body joints | 2.482 | 3.149 | 5.278 | 4.084 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
