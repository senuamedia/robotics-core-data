# youto22/so101_test_03

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [youto22/so101_test_03](https://huggingface.co/datasets/youto22/so101_test_03), revision `47b0d69007ff1ccd35cf9bcb1120c4d1e74e2b6e` |
| Author | Y-Kitamura ([youto22](https://huggingface.co/youto22)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 155 episodes, 119,442 frames |
| Rank | 149 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 884 | 909 |
| shoulder lift | 360/4,095 degree (degrees) | 2127 | 2146 |
| elbow flex | 360/4,095 degree (degrees) | 2182 | 2216 |
| wrist flex | 360/4,095 degree (degrees) | 1140 | 1202 |
| wrist roll | 360/4,095 degree (degrees) | 1944 | 1953 |
| gripper | 0.06028 of LeRobot's calibrated scale (a range of 1659 counts) | 899 | 709 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (58,474 frames); every frame then read as it would arrive. The held-out half (60,968 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 59 | 32 |
| shoulder lift | 31 | 102 | 17 |
| elbow flex | 49 | 99 | 4 |
| wrist flex | 33 | 114 | 8 |
| wrist roll | 44 | 137 | 2 |
| gripper | 30 | 93 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.554 | 0.870 | 1.663 | 1.241 |
| shoulder lift | 1.317 | 2.250 | 3.801 | 3.151 |
| elbow flex | 2.113 | 3.228 | 6.153 | 5.466 |
| wrist flex | 3.501 | 3.654 | 3.099 | 2.556 |
| wrist roll | 0.819 | 1.757 | 3.429 | 2.454 |
| gripper | 1.973 | 3.089 | 5.263 | 4.407 |
| five body joints | 1.971 | 2.556 | 3.909 | 3.283 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
