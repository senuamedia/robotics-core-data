# shinba/so101_red_cube_bowl_v2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [shinba/so101_red_cube_bowl_v2](https://huggingface.co/datasets/shinba/so101_red_cube_bowl_v2), revision `6dd77cbdf8b23ba604f529f29b45279d4f557f0c` |
| Author | [shinba](https://huggingface.co/shinba) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 144 episodes, 75,313 frames |
| Rank | 283 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 860 | 866 |
| shoulder lift | 360/4,095 degree (degrees) | 2046 | 2052 |
| elbow flex | 360/4,095 degree (degrees) | 2191 | 2218 |
| wrist flex | 360/4,095 degree (degrees) | 1058 | 1157 |
| wrist roll | 360/4,095 degree (degrees) | 973 | 991 |
| gripper | 0.06645 of LeRobot's calibrated scale (a range of 1505 counts) | 529 | 474 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (38,884 frames); every frame then read as it would arrive. The held-out half (36,429 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 45 | 79 | 1 |
| shoulder lift | 33 | 124 | 3 |
| elbow flex | 32 | 126 | 0 |
| wrist flex | 20 | 114 | 9 |
| wrist roll | 50 | 65 | 42 |
| gripper | 13 | 113 | 1 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.323 | 0.494 | 1.327 | 0.893 |
| shoulder lift | 2.169 | 2.939 | 4.158 | 2.961 |
| elbow flex | 7.408 | 8.690 | 10.611 | 9.318 |
| wrist flex | 1.030 | 1.245 | 2.243 | 1.705 |
| wrist roll | 0.342 | 0.271 | 0.821 | 0.545 |
| gripper | 7.077 | 7.076 | 7.221 | 7.083 |
| five body joints | 3.488 | 4.147 | 5.240 | 4.463 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
