# lepao/so101_test

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [lepao/so101_test](https://huggingface.co/datasets/lepao/so101_test), revision `3cf02f561d35cf3ed9e2be0f3662c18bc6d5515e` |
| Author | [lepao](https://huggingface.co/lepao) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 600 episodes, 198,164 frames |
| Rank | 79 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2212 | 2227 |
| shoulder lift | 360/4,095 degree (degrees) | 1904 | 1920 |
| elbow flex | 360/4,095 degree (degrees) | 1767 | 1780 |
| wrist flex | 360/4,095 degree (degrees) | 1071 | 1103 |
| wrist roll | 360/4,095 degree (degrees) | 1912 | 1959 |
| gripper | 0.06566 of LeRobot's calibrated scale (a range of 1523 counts) | 987 | 863 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (115,510 frames); every frame then read as it would arrive. The held-out half (82,654 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 68 | 204 | 42 |
| shoulder lift | 95 | 252 | 41 |
| elbow flex | 67 | 235 | 22 |
| wrist flex | 56 | 199 | 20 |
| wrist roll | 35 | 172 | 2 |
| gripper | 25 | 164 | 0 |

## The simulator against this arm

A development arm (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.410 | 0.839 | 2.192 | 1.391 |
| shoulder lift | 1.305 | 1.986 | 3.841 | 2.699 |
| elbow flex | 1.042 | 1.458 | 3.098 | 2.453 |
| wrist flex | 0.600 | 0.891 | 2.015 | 1.347 |
| wrist roll | 0.450 | 0.818 | 1.923 | 1.269 |
| gripper | 2.373 | 3.027 | 6.423 | 5.069 |
| five body joints | 0.838 | 1.283 | 2.716 | 1.931 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
