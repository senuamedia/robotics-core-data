# Atomictan/grab_tube_merged_v3

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Atomictan/grab_tube_merged_v3](https://huggingface.co/datasets/Atomictan/grab_tube_merged_v3), revision `e2b14afc747708b49fc97cfd2610b9ca093e1012` |
| Author | [Atomictan](https://huggingface.co/Atomictan) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 118 episodes, 95,305 frames |
| Rank | 210 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1954 | 1982 |
| shoulder lift | 360/4,095 degree (degrees) | 2022 | 2017 |
| elbow flex | 360/4,095 degree (degrees) | 1802 | 1880 |
| wrist flex | 360/4,095 degree (degrees) | 962 | 968 |
| wrist roll | 360/4,095 degree (degrees) | 2392 | 2409 |
| gripper | 0.06803 of LeRobot's calibrated scale (a range of 1470 counts) | 498 | 494 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (47,761 frames); every frame then read as it would arrive. The held-out half (47,544 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 54 | 98 | 0 |
| shoulder lift | 32 | 90 | 1 |
| elbow flex | 25 | 94 | 2 |
| wrist flex | 43 | 61 | 9 |
| wrist roll | 22 | 86 | 17 |
| gripper | 10 | 109 | 4 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.272 | 0.752 | 1.809 | 1.219 |
| shoulder lift | 0.914 | 1.038 | 1.424 | 1.114 |
| elbow flex | 1.157 | 1.434 | 2.367 | 2.128 |
| wrist flex | 2.533 | 2.557 | 1.097 | 0.876 |
| wrist roll | 0.339 | 0.484 | 1.043 | 0.805 |
| gripper | 5.826 | 5.890 | 5.959 | 5.820 |
| five body joints | 1.325 | 1.447 | 1.624 | 1.316 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
