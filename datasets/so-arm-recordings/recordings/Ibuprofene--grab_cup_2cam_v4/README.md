# Ibuprofene/grab_cup_2cam_v4

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Ibuprofene/grab_cup_2cam_v4](https://huggingface.co/datasets/Ibuprofene/grab_cup_2cam_v4), revision `6118206d65282132ca9a636fa06f6d031040c749` |
| Author | [Ibuprofene](https://huggingface.co/Ibuprofene) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 146 episodes, 104,819 frames |
| Rank | 187 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1486 | 1556 |
| shoulder lift | 360/4,095 degree (degrees) | 1821 | 1829 |
| elbow flex | 360/4,095 degree (degrees) | 1850 | 1848 |
| wrist flex | 360/4,095 degree (degrees) | 400 | 412 |
| wrist roll | 360/4,095 degree (degrees) | 365 | 399 |
| gripper | 0.03870 of LeRobot's calibrated scale (a range of 2584 counts) | 643 | 685 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (53,095 frames); every frame then read as it would arrive. The held-out half (51,724 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 27 | 110 | 13 |
| shoulder lift | 44 | 117 | 0 |
| elbow flex | 36 | 122 | 9 |
| wrist flex | 33 | 93 | 17 |
| wrist roll | 18 | 66 | 48 |
| gripper | 11 | 116 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 77.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.971 | 1.134 | 1.632 | 1.326 |
| shoulder lift | 1.557 | 1.662 | 1.380 | 1.146 |
| elbow flex | 1.277 | 1.764 | 3.678 | 3.258 |
| wrist flex | 0.463 | 0.436 | 0.459 | 0.346 |
| wrist roll | 0.181 | 0.179 | 0.426 | 0.414 |
| gripper | 25.440 | 25.486 | 17.739 | 17.632 |
| five body joints | 1.023 | 1.214 | 1.923 | 1.672 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
