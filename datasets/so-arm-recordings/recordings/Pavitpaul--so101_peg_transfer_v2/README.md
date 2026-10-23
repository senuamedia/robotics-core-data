# Pavitpaul/so101_peg_transfer_v2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Pavitpaul/so101_peg_transfer_v2](https://huggingface.co/datasets/Pavitpaul/so101_peg_transfer_v2), revision `05642f46ac4d4f75c5a207e4503926e5a7a46cc8` |
| Author | [Pavitpaul](https://huggingface.co/Pavitpaul) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 20 frames a second |
| Recording | 201 episodes, 74,437 frames |
| Rank | 294 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 738 | 752 |
| shoulder lift | 360/4,095 degree (degrees) | 1750 | 1759 |
| elbow flex | 360/4,095 degree (degrees) | 1741 | 1768 |
| wrist flex | 360/4,095 degree (degrees) | 769 | 791 |
| wrist roll | 360/4,095 degree (degrees) | 354 | 368 |
| gripper | 0.06711 of LeRobot's calibrated scale (a range of 1490 counts) | 389 | 343 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (39,303 frames); every frame then read as it would arrive. The held-out half (35,134 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 41 | 80 | 12 |
| shoulder lift | 18 | 182 | 8 |
| elbow flex | 38 | 149 | 5 |
| wrist flex | 21 | 117 | 18 |
| wrist roll | 41 | 62 | 7 |
| gripper | 5 | 161 | 3 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 399 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 37.5 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.186 | 0.254 | 0.667 | 0.333 |
| shoulder lift | 1.868 | 2.377 | 3.173 | 1.900 |
| elbow flex | 1.374 | 1.905 | 3.064 | 1.924 |
| wrist flex | 1.487 | 1.646 | 1.690 | 0.968 |
| wrist roll | 0.110 | 0.080 | 0.306 | 0.235 |
| gripper | 3.477 | 3.525 | 3.712 | 3.451 |
| five body joints | 1.235 | 1.553 | 2.137 | 1.297 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
