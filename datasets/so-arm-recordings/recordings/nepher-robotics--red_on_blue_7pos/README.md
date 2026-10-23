# nepher-robotics/red_on_blue_7pos

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [nepher-robotics/red_on_blue_7pos](https://huggingface.co/datasets/nepher-robotics/red_on_blue_7pos), revision `777a1f239ac6698e8580b5dc0a286b39a5db37ab` |
| Author | [nepher-robotics](https://huggingface.co/nepher-robotics) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 140 episodes, 104,859 frames |
| Rank | 186 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1292 | 1305 |
| shoulder lift | 360/4,095 degree (degrees) | 1750 | 1762 |
| elbow flex | 360/4,095 degree (degrees) | 1676 | 1726 |
| wrist flex | 360/4,095 degree (degrees) | 798 | 830 |
| wrist roll | 360/4,095 degree (degrees) | 1361 | 1355 |
| gripper | 0.06849 of LeRobot's calibrated scale (a range of 1460 counts) | 387 | 487 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (52,429 frames); every frame then read as it would arrive. The held-out half (52,430 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 27 | 84 | 13 |
| shoulder lift | 19 | 139 | 38 |
| elbow flex | 22 | 127 | 27 |
| wrist flex | 45 | 79 | 9 |
| wrist roll | 34 | 80 | 13 |
| gripper | 10 | 85 | 5 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.341 | 0.733 | 1.655 | 1.082 |
| shoulder lift | 1.256 | 2.016 | 3.603 | 2.630 |
| elbow flex | 1.187 | 1.614 | 2.941 | 2.259 |
| wrist flex | 0.548 | 0.622 | 1.201 | 0.831 |
| wrist roll | 0.491 | 0.609 | 1.524 | 0.987 |
| gripper | 7.168 | 7.176 | 7.201 | 7.116 |
| five body joints | 0.853 | 1.262 | 2.372 | 1.723 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
