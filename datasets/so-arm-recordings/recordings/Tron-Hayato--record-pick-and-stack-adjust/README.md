# Tron-Hayato/record-pick-and-stack-adjust

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Tron-Hayato/record-pick-and-stack-adjust](https://huggingface.co/datasets/Tron-Hayato/record-pick-and-stack-adjust), revision `42f550eb3a1419ba6690b778d71737f3fbbdcae2` |
| Author | Hayato Nakamura ([Tron-Hayato](https://huggingface.co/Tron-Hayato)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 115,802 frames |
| Rank | 158 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 947 | 967 |
| shoulder lift | 360/4,095 degree (degrees) | 1774 | 1777 |
| elbow flex | 360/4,095 degree (degrees) | 1612 | 1644 |
| wrist flex | 360/4,095 degree (degrees) | 323 | 501 |
| wrist roll | 360/4,095 degree (degrees) | 883 | 908 |
| gripper | 0.06854 of LeRobot's calibrated scale (a range of 1459 counts) | 750 | 623 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (89,409 frames); every frame then read as it would arrive. The held-out half (26,393 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 45 | 66 | 3 |
| shoulder lift | 55 | 72 | 3 |
| elbow flex | 43 | 69 | 2 |
| wrist flex | 42 | 57 | 0 |
| wrist roll | 56 | 100 | 3 |
| gripper | 81 | 100 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.239 | 0.174 | 0.678 | 0.478 |
| shoulder lift | 1.458 | 1.640 | 2.231 | 1.713 |
| elbow flex | 0.786 | 1.019 | 3.019 | 2.619 |
| wrist flex | 4.439 | 4.449 | 5.105 | 5.087 |
| wrist roll | 0.236 | 0.174 | 0.674 | 0.457 |
| gripper | 0.374 | 0.544 | 1.251 | 0.820 |
| five body joints | 2.124 | 2.171 | 2.866 | 2.686 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
