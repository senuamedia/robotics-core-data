# Deviant65/so101_redcube_3new

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Deviant65/so101_redcube_3new](https://huggingface.co/datasets/Deviant65/so101_redcube_3new), revision `bc1309447aa0c98c03fd20120a50a156cc57c279` |
| Author | dev ([Deviant65](https://huggingface.co/Deviant65)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 143 episodes, 132,974 frames |
| Rank | 132 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1540 | 1557 |
| shoulder lift | 360/4,095 degree (degrees) | 2051 | 2061 |
| elbow flex | 360/4,095 degree (degrees) | 1966 | 2092 |
| wrist flex | 360/4,095 degree (degrees) | 954 | 981 |
| wrist roll | 360/4,095 degree (degrees) | 1852 | 1945 |
| gripper | 0.06562 of LeRobot's calibrated scale (a range of 1524 counts) | 896 | 806 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (60,134 frames); every frame then read as it would arrive. The held-out half (72,840 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 31 | 76 | 3 |
| shoulder lift | 42 | 105 | 51 |
| elbow flex | 38 | 98 | 6 |
| wrist flex | 29 | 83 | 22 |
| wrist roll | 26 | 85 | 11 |
| gripper | 16 | 121 | 3 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.498 | 0.591 | 1.045 | 0.849 |
| shoulder lift | 1.908 | 2.100 | 1.906 | 1.671 |
| elbow flex | 1.739 | 1.860 | 3.795 | 3.511 |
| wrist flex | 0.195 | 0.198 | 0.415 | 0.405 |
| wrist roll | 0.370 | 0.434 | 1.021 | 0.745 |
| gripper | 0.374 | 0.624 | 1.350 | 0.912 |
| five body joints | 1.190 | 1.299 | 2.017 | 1.819 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
