# Oscarcarrh/so101-cables-vla

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Oscarcarrh/so101-cables-vla](https://huggingface.co/datasets/Oscarcarrh/so101-cables-vla), revision `13b87e562efe3014c66f65d3c73571515c566f28` |
| Author | Oscar Carranza Hernándes ([Oscarcarrh](https://huggingface.co/Oscarcarrh)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 300 episodes, 169,721 frames |
| Rank | 100 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1010 | 1022 |
| shoulder lift | 360/4,095 degree (degrees) | 2078 | 2112 |
| elbow flex | 360/4,095 degree (degrees) | 2118 | 2189 |
| wrist flex | 360/4,095 degree (degrees) | 1988 | 2182 |
| wrist roll | 360/4,095 degree (degrees) | 1967 | 1982 |
| gripper | 0.07042 of LeRobot's calibrated scale (a range of 1420 counts) | 382 | 380 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (137,393 frames); every frame then read as it would arrive. The held-out half (136,495 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 649 | 957 | 253 |
| shoulder lift | 1103 | 1543 | 707 |
| elbow flex | 984 | 1409 | 383 |
| wrist flex | 724 | 1749 | 624 |
| wrist roll | 845 | 1200 | 122 |
| gripper | 360 | 418 | 61 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 77.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.679 | 0.982 | 1.805 | 1.316 |
| shoulder lift | 1.501 | 2.312 | 3.834 | 3.237 |
| elbow flex | 1.324 | 1.930 | 3.924 | 3.436 |
| wrist flex | 1.272 | 2.505 | 4.259 | 3.266 |
| wrist roll | 0.587 | 0.771 | 1.523 | 1.132 |
| gripper | 2.002 | 2.031 | 2.657 | 2.289 |
| five body joints | 1.133 | 1.838 | 3.280 | 2.681 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
