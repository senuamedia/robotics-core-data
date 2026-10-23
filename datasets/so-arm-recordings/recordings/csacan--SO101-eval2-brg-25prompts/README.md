# csacan/SO101-eval2-brg-25prompts

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [csacan/SO101-eval2-brg-25prompts](https://huggingface.co/datasets/csacan/SO101-eval2-brg-25prompts), revision `6583cba56855a2d4f542b4d87c1990a8786a30af` |
| Author | [csacan](https://huggingface.co/csacan) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 250 episodes, 112,015 frames |
| Rank | 168 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 704 | 730 |
| shoulder lift | 360/4,095 degree (degrees) | 1276 | 1291 |
| elbow flex | 360/4,095 degree (degrees) | 637 | 714 |
| wrist flex | 360/4,095 degree (degrees) | 847 | 876 |
| wrist roll | 360/4,095 degree (degrees) | 1188 | 1199 |
| gripper | 0.06978 of LeRobot's calibrated scale (a range of 1433 counts) | 508 | 486 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (55,743 frames); every frame then read as it would arrive. The held-out half (56,272 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 31 | 60 | 0 |
| shoulder lift | 61 | 103 | 0 |
| elbow flex | 36 | 82 | 0 |
| wrist flex | 48 | 89 | 0 |
| wrist roll | 57 | 91 | 0 |
| gripper | 27 | 107 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.260 | 0.329 | 0.918 | 0.619 |
| shoulder lift | 1.593 | 1.837 | 2.366 | 1.567 |
| elbow flex | 1.256 | 1.401 | 2.209 | 1.901 |
| wrist flex | 1.609 | 1.645 | 1.629 | 1.131 |
| wrist roll | 0.302 | 0.732 | 2.281 | 1.340 |
| gripper | 1.891 | 1.997 | 2.534 | 2.114 |
| five body joints | 1.171 | 1.318 | 1.958 | 1.379 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
