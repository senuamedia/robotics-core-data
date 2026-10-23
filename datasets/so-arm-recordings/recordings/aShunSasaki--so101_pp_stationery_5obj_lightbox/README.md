# aShunSasaki/so101_pp_stationery_5obj_lightbox

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [aShunSasaki/so101_pp_stationery_5obj_lightbox](https://huggingface.co/datasets/aShunSasaki/so101_pp_stationery_5obj_lightbox), revision `0aa43416f64b1a57188f69bffc1e95a0d049ba31` |
| Author | Shun Sasaki ([aShunSasaki](https://huggingface.co/aShunSasaki)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 416 episodes, 305,227 frames |
| Rank | 40 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1909 | 1965 |
| shoulder lift | 360/4,095 degree (degrees) | 1935 | 1952 |
| elbow flex | 360/4,095 degree (degrees) | 1835 | 1884 |
| wrist flex | 360/4,095 degree (degrees) | 991 | 1120 |
| wrist roll | 360/4,095 degree (degrees) | 2045 | 2079 |
| gripper | 0.06693 of LeRobot's calibrated scale (a range of 1494 counts) | 528 | 490 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (156,763 frames); every frame then read as it would arrive. The held-out half (148,464 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 57 | 73 | 3 |
| shoulder lift | 73 | 87 | 4 |
| elbow flex | 75 | 79 | 0 |
| wrist flex | 57 | 77 | 3 |
| wrist roll | 77 | 94 | 1 |
| gripper | 39 | 88 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.760 | 1.109 | 1.975 | 1.472 |
| shoulder lift | 1.785 | 2.622 | 4.797 | 4.151 |
| elbow flex | 1.243 | 2.047 | 3.832 | 3.174 |
| wrist flex | 3.430 | 3.525 | 3.624 | 3.401 |
| wrist roll | 0.392 | 0.636 | 1.314 | 0.923 |
| gripper | 3.408 | 3.490 | 3.733 | 3.485 |
| five body joints | 1.856 | 2.242 | 3.360 | 2.894 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
