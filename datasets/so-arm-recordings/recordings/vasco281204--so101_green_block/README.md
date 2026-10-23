# vasco281204/so101_green_block

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [vasco281204/so101_green_block](https://huggingface.co/datasets/vasco281204/so101_green_block), revision `0a45e12ec9282a939612298f83db435022b09087` |
| Author | Nguyen Viet Anh ([vasco281204](https://huggingface.co/vasco281204)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 170,639 frames |
| Rank | 97 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2269 | 2281 |
| shoulder lift | 360/4,095 degree (degrees) | 1724 | 1732 |
| elbow flex | 360/4,095 degree (degrees) | 1411 | 1525 |
| wrist flex | 360/4,095 degree (degrees) | 1560 | 1581 |
| wrist roll | 360/4,095 degree (degrees) | 2294 | 2319 |
| gripper | 0.06752 of LeRobot's calibrated scale (a range of 1481 counts) | 983 | 866 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (90,885 frames); every frame then read as it would arrive. The held-out half (79,754 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 67 | 77 | 26 |
| shoulder lift | 79 | 92 | 19 |
| elbow flex | 65 | 78 | 9 |
| wrist flex | 37 | 91 | 19 |
| wrist roll | 29 | 91 | 0 |
| gripper | 24 | 90 | 5 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 61.1 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.216 | 0.236 | 0.626 | 0.482 |
| shoulder lift | 0.972 | 1.414 | 2.439 | 2.019 |
| elbow flex | 1.544 | 1.888 | 3.845 | 3.629 |
| wrist flex | 0.878 | 1.118 | 2.107 | 1.774 |
| wrist roll | 0.340 | 0.381 | 0.936 | 0.673 |
| gripper | 2.629 | 2.932 | 3.538 | 3.129 |
| five body joints | 0.922 | 1.184 | 2.299 | 2.053 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
