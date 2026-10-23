# Zekai-Chen/so101_pi05_pick_place_3cam

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Zekai-Chen/so101_pi05_pick_place_3cam](https://huggingface.co/datasets/Zekai-Chen/so101_pi05_pick_place_3cam), revision `af80dc0e6cff91841486dbd20d2725d755c922b5` |
| Author | [Zekai-Chen](https://huggingface.co/Zekai-Chen) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 300 episodes, 111,420 frames |
| Rank | 171 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1480 | 1513 |
| shoulder lift | 360/4,095 degree (degrees) | 2006 | 2011 |
| elbow flex | 360/4,095 degree (degrees) | 2003 | 2045 |
| wrist flex | 360/4,095 degree (degrees) | 1108 | 1269 |
| wrist roll | 360/4,095 degree (degrees) | 1540 | 2041 |
| gripper | 0.06840 of LeRobot's calibrated scale (a range of 1462 counts) | 869 | 764 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (57,544 frames); every frame then read as it would arrive. The held-out half (53,876 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 49 | 159 | 4 |
| shoulder lift | 57 | 237 | 17 |
| elbow flex | 53 | 228 | 12 |
| wrist flex | 46 | 172 | 15 |
| wrist roll | 40 | 184 | 29 |
| gripper | 23 | 124 | 13 |

## The simulator against this arm

A development arm (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.332 | 0.781 | 2.068 | 1.337 |
| shoulder lift | 1.385 | 1.860 | 3.871 | 2.944 |
| elbow flex | 1.063 | 1.339 | 3.570 | 2.817 |
| wrist flex | 3.500 | 3.552 | 5.588 | 5.424 |
| wrist roll | 0.414 | 0.624 | 1.590 | 1.025 |
| gripper | 11.022 | 11.028 | 11.661 | 11.309 |
| five body joints | 1.764 | 1.942 | 3.626 | 3.126 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
