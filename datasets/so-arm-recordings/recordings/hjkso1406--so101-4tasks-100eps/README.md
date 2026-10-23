# hjkso1406/so101-4tasks-100eps

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [hjkso1406/so101-4tasks-100eps](https://huggingface.co/datasets/hjkso1406/so101-4tasks-100eps), revision `52df842eec7e9425a87040528f94ab20732e42fb` |
| Author | Heo ([hjkso1406](https://huggingface.co/hjkso1406)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 400 episodes, 247,823 frames |
| Rank | 54 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2161 | 2182 |
| shoulder lift | 360/4,095 degree (degrees) | 2329 | 2337 |
| elbow flex | 360/4,095 degree (degrees) | 2266 | 2404 |
| wrist flex | 360/4,095 degree (degrees) | 1485 | 1525 |
| wrist roll | 360/4,095 degree (degrees) | 1958 | 1980 |
| gripper | 0.06757 of LeRobot's calibrated scale (a range of 1480 counts) | 786 | 687 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (104,553 frames); every frame then read as it would arrive. The held-out half (143,270 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 57 | 256 | 39 |
| shoulder lift | 89 | 253 | 31 |
| elbow flex | 56 | 265 | 24 |
| wrist flex | 74 | 252 | 5 |
| wrist roll | 36 | 169 | 554 |
| gripper | 31 | 164 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.412 | 0.855 | 1.995 | 1.329 |
| shoulder lift | 2.145 | 2.470 | 3.315 | 2.470 |
| elbow flex | 0.677 | 0.737 | 3.350 | 2.912 |
| wrist flex | 0.877 | 0.986 | 1.466 | 1.145 |
| wrist roll | 0.274 | 0.303 | 0.888 | 0.631 |
| gripper | 3.591 | 3.601 | 3.990 | 3.630 |
| five body joints | 1.102 | 1.299 | 2.413 | 1.900 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
