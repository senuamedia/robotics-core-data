# poi69420/so101_red_screwdriver_to_yellow_container_extra_02

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [poi69420/so101_red_screwdriver_to_yellow_container_extra_02](https://huggingface.co/datasets/poi69420/so101_red_screwdriver_to_yellow_container_extra_02), revision `09f6c7ad19479ec9b075176368c1c9226cdb358b` |
| Author | [poi69420](https://huggingface.co/poi69420) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 136 episodes, 80,838 frames |
| Rank | 265 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 816 | 840 |
| shoulder lift | 360/4,095 degree (degrees) | 1587 | 1597 |
| elbow flex | 360/4,095 degree (degrees) | 1338 | 1526 |
| wrist flex | 360/4,095 degree (degrees) | 833 | 857 |
| wrist roll | 360/4,095 degree (degrees) | 643 | 656 |
| gripper | 0.06906 of LeRobot's calibrated scale (a range of 1448 counts) | 395 | 328 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (40,430 frames); every frame then read as it would arrive. The held-out half (40,408 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 34 | 67 | 3 |
| shoulder lift | 50 | 79 | 83 |
| elbow flex | 36 | 91 | 0 |
| wrist flex | 33 | 65 | 20 |
| wrist roll | 14 | 63 | 6 |
| gripper | 20 | 111 | 4 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.268 | 0.571 | 1.394 | 0.940 |
| shoulder lift | 1.327 | 1.597 | 2.672 | 1.972 |
| elbow flex | 7.872 | 7.918 | 6.387 | 6.195 |
| wrist flex | 0.306 | 0.397 | 1.059 | 0.782 |
| wrist roll | 0.372 | 0.425 | 0.950 | 0.680 |
| gripper | 2.295 | 2.347 | 2.813 | 2.519 |
| five body joints | 3.578 | 3.630 | 3.222 | 2.974 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
