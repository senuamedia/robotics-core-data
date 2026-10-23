# aivanni/so101-puzzle-v8

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [aivanni/so101-puzzle-v8](https://huggingface.co/datasets/aivanni/so101-puzzle-v8), revision `9fe8495800cc76b97bbd5bd73d191808836f1b47` |
| Author | Andrey I ([aivanni](https://huggingface.co/aivanni)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 164 episodes, 174,472 frames |
| Rank | 95 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 886 | 911 |
| shoulder lift | 360/4,095 degree (degrees) | 1890 | 1901 |
| elbow flex | 360/4,095 degree (degrees) | 1752 | 1773 |
| wrist flex | 360/4,095 degree (degrees) | 1341 | 1342 |
| wrist roll | 360/4,095 degree (degrees) | 1757 | 1754 |
| gripper | 0.06882 of LeRobot's calibrated scale (a range of 1453 counts) | 333 | 287 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (90,534 frames); every frame then read as it would arrive. The held-out half (83,938 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 36 | 52 | 20 |
| shoulder lift | 70 | 86 | 36 |
| elbow flex | 60 | 95 | 5 |
| wrist flex | 38 | 95 | 32 |
| wrist roll | 54 | 138 | 11 |
| gripper | 12 | 99 | 7 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.254 | 0.307 | 0.841 | 0.571 |
| shoulder lift | 1.974 | 2.139 | 2.242 | 1.928 |
| elbow flex | 1.205 | 1.248 | 2.425 | 2.156 |
| wrist flex | 0.516 | 0.610 | 1.350 | 1.061 |
| wrist roll | 0.393 | 0.745 | 1.842 | 1.172 |
| gripper | 2.606 | 2.618 | 2.654 | 2.557 |
| five body joints | 1.080 | 1.196 | 1.834 | 1.495 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
