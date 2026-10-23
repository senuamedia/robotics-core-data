# trivien/so101-pick-place1_20260819_144049

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [trivien/so101-pick-place1_20260819_144049](https://huggingface.co/datasets/trivien/so101-pick-place1_20260819_144049), revision `3273c7950141f51de6ed652ebfef9f0d70084a0b` |
| Author | [trivien](https://huggingface.co/trivien) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 70 episodes, 93,859 frames |
| Rank | 216 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1562 | 1590 |
| shoulder lift | 360/4,095 degree (degrees) | 1651 | 1660 |
| elbow flex | 360/4,095 degree (degrees) | 1424 | 1520 |
| wrist flex | 360/4,095 degree (degrees) | 872 | 910 |
| wrist roll | 360/4,095 degree (degrees) | 1947 | 1987 |
| gripper | 0.06684 of LeRobot's calibrated scale (a range of 1496 counts) | 530 | 484 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (46,928 frames); every frame then read as it would arrive. The held-out half (46,931 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 53 | 58 | 41 |
| shoulder lift | 36 | 64 | 23 |
| elbow flex | 28 | 64 | 89 |
| wrist flex | 33 | 49 | 59 |
| wrist roll | 36 | 93 | 1 |
| gripper | 12 | 79 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 72.2 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.467 | 0.655 | 1.436 | 1.027 |
| shoulder lift | 1.261 | 1.610 | 2.891 | 2.703 |
| elbow flex | 2.138 | 2.801 | 5.505 | 5.056 |
| wrist flex | 0.906 | 1.250 | 2.080 | 1.727 |
| wrist roll | 1.229 | 2.010 | 3.027 | 2.395 |
| gripper | 1.658 | 1.712 | 1.947 | 1.810 |
| five body joints | 1.320 | 1.814 | 3.293 | 2.920 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
