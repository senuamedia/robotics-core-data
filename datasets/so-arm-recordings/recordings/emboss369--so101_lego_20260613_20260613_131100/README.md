# emboss369/so101_lego_20260613_20260613_131100

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [emboss369/so101_lego_20260613_20260613_131100](https://huggingface.co/datasets/emboss369/so101_lego_20260613_20260613_131100), revision `19002ef4e25b738a944439c4a6d0ede0421a5383` |
| Author | Hiroaki Kaneda ([emboss369](https://huggingface.co/emboss369)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 201 episodes, 119,104 frames |
| Rank | 150 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1245 | 1251 |
| shoulder lift | 360/4,095 degree (degrees) | 1941 | 1963 |
| elbow flex | 360/4,095 degree (degrees) | 1321 | 1363 |
| wrist flex | 360/4,095 degree (degrees) | 1898 | 1907 |
| wrist roll | 360/4,095 degree (degrees) | 1702 | 1740 |
| gripper | 0.06803 of LeRobot's calibrated scale (a range of 1470 counts) | 1304 | 1279 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (54,953 frames); every frame then read as it would arrive. The held-out half (64,151 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 18 | 120 | 14 |
| shoulder lift | 38 | 145 | 26 |
| elbow flex | 51 | 115 | 11 |
| wrist flex | 58 | 155 | 23 |
| wrist roll | 30 | 176 | 24 |
| gripper | 29 | 123 | 7 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.515 | 0.839 | 2.030 | 1.299 |
| shoulder lift | 1.698 | 1.984 | 2.839 | 1.883 |
| elbow flex | 1.283 | 1.429 | 2.272 | 1.768 |
| wrist flex | 0.621 | 1.169 | 2.836 | 1.836 |
| wrist roll | 0.607 | 1.339 | 3.019 | 1.955 |
| gripper | 2.459 | 2.562 | 8.396 | 7.301 |
| five body joints | 1.053 | 1.402 | 2.627 | 1.763 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
