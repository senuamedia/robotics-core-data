# lmsforAI/so101_cube_pick_place_dualcam_v2_20260603_063542

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [lmsforAI/so101_cube_pick_place_dualcam_v2_20260603_063542](https://huggingface.co/datasets/lmsforAI/so101_cube_pick_place_dualcam_v2_20260603_063542), revision `be9b3526d3b30df884a030c1de5b755e8fe0a373` |
| Author | [lmsforAI](https://huggingface.co/lmsforAI) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 150 episodes, 103,964 frames |
| Rank | 191 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1652 | 1668 |
| shoulder lift | 360/4,095 degree (degrees) | 1920 | 1979 |
| elbow flex | 360/4,095 degree (degrees) | 2052 | 2086 |
| wrist flex | 360/4,095 degree (degrees) | 596 | 704 |
| wrist roll | 360/4,095 degree (degrees) | 968 | 996 |
| gripper | 0.06826 of LeRobot's calibrated scale (a range of 1465 counts) | 407 | 501 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (50,224 frames); every frame then read as it would arrive. The held-out half (53,740 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 49 | 98 | 23 |
| shoulder lift | 55 | 138 | 16 |
| elbow flex | 35 | 140 | 19 |
| wrist flex | 42 | 105 | 5 |
| wrist roll | 37 | 110 | 11 |
| gripper | 24 | 154 | 6 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.308 | 0.529 | 1.428 | 0.946 |
| shoulder lift | 3.375 | 3.566 | 3.578 | 3.272 |
| elbow flex | 0.865 | 0.925 | 2.823 | 2.362 |
| wrist flex | 2.357 | 2.378 | 3.651 | 3.625 |
| wrist roll | 0.406 | 0.479 | 1.019 | 0.747 |
| gripper | 3.160 | 3.183 | 3.206 | 3.106 |
| five body joints | 1.894 | 1.986 | 2.726 | 2.484 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
