# binhpham/naive-bench

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [binhpham/naive-bench](https://huggingface.co/datasets/binhpham/naive-bench), revision `21bb67a121d11031495b7b98d21a65e4e02a6430` |
| Author | [binhpham](https://huggingface.co/binhpham) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 312 episodes, 115,378 frames |
| Rank | 163 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1382 | 1407 |
| shoulder lift | 360/4,095 degree (degrees) | 2098 | 2094 |
| elbow flex | 360/4,095 degree (degrees) | 2013 | 2098 |
| wrist flex | 360/4,095 degree (degrees) | 1363 | 1416 |
| wrist roll | 360/4,095 degree (degrees) | 1975 | 1994 |
| gripper | 0.06439 of LeRobot's calibrated scale (a range of 1553 counts) | 648 | 551 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (59,757 frames); every frame then read as it would arrive. The held-out half (55,621 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 46 | 180 | 11 |
| shoulder lift | 48 | 373 | 36 |
| elbow flex | 39 | 311 | 43 |
| wrist flex | 25 | 241 | 29 |
| wrist roll | 26 | 207 | 30 |
| gripper | 26 | 216 | 31 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 116.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 1.390 | 2.103 | 2.571 | 2.329 |
| shoulder lift | 6.345 | 10.770 | 13.488 | 12.015 |
| elbow flex | 5.701 | 8.568 | 11.487 | 10.509 |
| wrist flex | 1.549 | 3.099 | 4.030 | 3.564 |
| wrist roll | 0.955 | 1.473 | 1.850 | 1.650 |
| gripper | 9.776 | 10.325 | 10.732 | 10.576 |
| five body joints | 3.949 | 6.412 | 8.247 | 7.425 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
