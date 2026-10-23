# sajad2222/so101_semantic_pick_place_4cmd

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [sajad2222/so101_semantic_pick_place_4cmd](https://huggingface.co/datasets/sajad2222/so101_semantic_pick_place_4cmd), revision `9f2d45f61e28b2772b0ebfa618218f92076b2a59` |
| Author | [sajad2222](https://huggingface.co/sajad2222) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 84,170 frames |
| Rank | 255 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1000 | 1033 |
| shoulder lift | 360/4,095 degree (degrees) | 1996 | 2004 |
| elbow flex | 360/4,095 degree (degrees) | 2002 | 2101 |
| wrist flex | 360/4,095 degree (degrees) | 1330 | 1358 |
| wrist roll | 360/4,095 degree (degrees) | 666 | 672 |
| gripper | 0.06807 of LeRobot's calibrated scale (a range of 1469 counts) | 1378 | 1216 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (47,821 frames); every frame then read as it would arrive. The held-out half (36,349 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 50 | 70 | 85 |
| shoulder lift | 55 | 106 | 200 |
| elbow flex | 56 | 113 | 243 |
| wrist flex | 17 | 105 | 114 |
| wrist roll | 59 | 61 | 60 |
| gripper | 29 | 115 | 41 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.412 | 0.626 | 1.279 | 0.942 |
| shoulder lift | 1.137 | 1.680 | 3.183 | 2.216 |
| elbow flex | 1.461 | 2.040 | 3.748 | 3.000 |
| wrist flex | 0.754 | 1.495 | 3.114 | 2.134 |
| wrist roll | 0.283 | 0.280 | 0.652 | 0.476 |
| gripper | 10.165 | 10.227 | 10.464 | 10.224 |
| five body joints | 0.921 | 1.392 | 2.680 | 1.979 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
