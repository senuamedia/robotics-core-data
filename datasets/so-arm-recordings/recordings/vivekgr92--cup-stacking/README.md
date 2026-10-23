# vivekgr92/cup-stacking

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [vivekgr92/cup-stacking](https://huggingface.co/datasets/vivekgr92/cup-stacking), revision `be3a0a0cf2d8e0166f97ee87b3abb15cd1c39156` |
| Author | [vivekgr92](https://huggingface.co/vivekgr92) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 86 episodes, 77,284 frames |
| Rank | 279 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 674 | 679 |
| shoulder lift | 360/4,095 degree (degrees) | 1248 | 1252 |
| elbow flex | 360/4,095 degree (degrees) | 1556 | 1594 |
| wrist flex | 360/4,095 degree (degrees) | 2335 | 2350 |
| wrist roll | 360/4,095 degree (degrees) | 2035 | 2054 |
| gripper | 0.06309 of LeRobot's calibrated scale (a range of 1585 counts) | 1157 | 999 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (38,653 frames); every frame then read as it would arrive. The held-out half (38,631 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 35 | 47 | 7 |
| shoulder lift | 31 | 80 | 6 |
| elbow flex | 26 | 99 | 11 |
| wrist flex | 28 | 158 | 8 |
| wrist roll | 21 | 125 | 17 |
| gripper | 18 | 119 | 3 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.350 | 0.295 | 0.925 | 0.575 |
| shoulder lift | 0.879 | 1.059 | 2.031 | 1.454 |
| elbow flex | 1.061 | 1.485 | 3.041 | 2.184 |
| wrist flex | 2.285 | 2.892 | 4.347 | 2.877 |
| wrist roll | 0.642 | 1.271 | 2.931 | 1.861 |
| gripper | 0.691 | 1.184 | 2.828 | 1.844 |
| five body joints | 1.237 | 1.636 | 2.888 | 1.946 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
