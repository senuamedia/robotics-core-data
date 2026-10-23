# joanfox/so101_pencil_merged_199_hil

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [joanfox/so101_pencil_merged_199_hil](https://huggingface.co/datasets/joanfox/so101_pencil_merged_199_hil), revision `ea1f4e415b78ac3bd799cbd0e10902c9f10d190c` |
| Author | jojo joan ([joanfox](https://huggingface.co/joanfox)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 199 episodes, 185,140 frames |
| Rank | 89 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1442 | 1455 |
| shoulder lift | 360/4,095 degree (degrees) | 1869 | 1871 |
| elbow flex | 360/4,095 degree (degrees) | 1773 | 1805 |
| wrist flex | 360/4,095 degree (degrees) | 1439 | 1478 |
| wrist roll | 360/4,095 degree (degrees) | 2107 | 2159 |
| gripper | 0.07072 of LeRobot's calibrated scale (a range of 1414 counts) | 921 | 833 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (96,881 frames); every frame then read as it would arrive. The held-out half (88,259 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 60 | 72 | 11 |
| shoulder lift | 67 | 78 | 29 |
| elbow flex | 59 | 75 | 53 |
| wrist flex | 64 | 99 | 14 |
| wrist roll | 46 | 91 | 56 |
| gripper | 23 | 114 | 4 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.333 | 0.525 | 1.579 | 0.979 |
| shoulder lift | 1.567 | 2.115 | 3.557 | 2.450 |
| elbow flex | 1.188 | 1.680 | 3.579 | 2.556 |
| wrist flex | 0.863 | 1.663 | 3.562 | 2.324 |
| wrist roll | 0.463 | 0.698 | 1.625 | 1.105 |
| gripper | 2.603 | 2.822 | 3.829 | 3.096 |
| five body joints | 0.994 | 1.471 | 2.942 | 2.005 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
