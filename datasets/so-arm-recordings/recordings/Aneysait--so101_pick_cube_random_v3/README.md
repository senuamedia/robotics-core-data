# Aneysait/so101_pick_cube_random_v3

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Aneysait/so101_pick_cube_random_v3](https://huggingface.co/datasets/Aneysait/so101_pick_cube_random_v3), revision `785b171349c5b1735ff8b5e1e381bc8849d016aa` |
| Author | [Aneysait](https://huggingface.co/Aneysait) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 198 episodes, 86,249 frames |
| Rank | 247 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2256 | 2286 |
| shoulder lift | 360/4,095 degree (degrees) | 1938 | 1941 |
| elbow flex | 360/4,095 degree (degrees) | 1797 | 1800 |
| wrist flex | 360/4,095 degree (degrees) | 765 | 809 |
| wrist roll | 360/4,095 degree (degrees) | 1243 | 1294 |
| gripper | 0.06798 of LeRobot's calibrated scale (a range of 1471 counts) | 1093 | 970 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,018 frames); every frame then read as it would arrive. The held-out half (42,231 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 35 | 116 | 132 |
| shoulder lift | 42 | 164 | 28 |
| elbow flex | 19 | 148 | 20 |
| wrist flex | 37 | 91 | 31 |
| wrist roll | 20 | 90 | 12 |
| gripper | 21 | 128 | 29 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.473 | 1.139 | 2.300 | 1.628 |
| shoulder lift | 0.981 | 2.286 | 4.501 | 3.284 |
| elbow flex | 1.758 | 2.764 | 4.812 | 3.726 |
| wrist flex | 1.030 | 1.219 | 1.886 | 1.333 |
| wrist roll | 1.281 | 1.838 | 2.721 | 2.197 |
| gripper | 3.784 | 4.148 | 7.405 | 6.323 |
| five body joints | 1.181 | 1.951 | 3.454 | 2.605 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
