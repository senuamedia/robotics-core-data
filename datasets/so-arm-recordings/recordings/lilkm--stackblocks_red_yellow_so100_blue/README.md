# lilkm/stackblocks_red_yellow_so100_blue

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [lilkm/stackblocks_red_yellow_so100_blue](https://huggingface.co/datasets/lilkm/stackblocks_red_yellow_so100_blue), revision `0a8d31eb55529b37286f3330e171c0475f489884` |
| Author | [lilkm](https://huggingface.co/lilkm) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 79,781 frames |
| Rank | 269 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1115 | 1134 |
| shoulder lift | 360/4,095 degree (degrees) | 1798 | 1813 |
| elbow flex | 360/4,095 degree (degrees) | 1724 | 1759 |
| wrist flex | 360/4,095 degree (degrees) | 654 | 680 |
| wrist roll | 360/4,095 degree (degrees) | 1426 | 1440 |
| gripper | 0.06920 of LeRobot's calibrated scale (a range of 1445 counts) | 674 | 591 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (42,831 frames); every frame then read as it would arrive. The held-out half (36,950 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 36 | 56 | 8 |
| shoulder lift | 67 | 102 | 18 |
| elbow flex | 42 | 105 | 1 |
| wrist flex | 67 | 76 | 15 |
| wrist roll | 40 | 74 | 51 |
| gripper | 25 | 114 | 79 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 22.2 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.259 | 0.265 | 0.799 | 0.527 |
| shoulder lift | 1.649 | 1.820 | 2.313 | 1.834 |
| elbow flex | 0.530 | 0.691 | 2.915 | 2.568 |
| wrist flex | 0.333 | 0.344 | 0.792 | 0.576 |
| wrist roll | 0.244 | 0.465 | 1.518 | 0.902 |
| gripper | 9.918 | 9.941 | 10.125 | 10.011 |
| five body joints | 0.805 | 0.915 | 1.866 | 1.508 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
