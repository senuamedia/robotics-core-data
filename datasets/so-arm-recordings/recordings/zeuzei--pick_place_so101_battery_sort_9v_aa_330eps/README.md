# zeuzei/pick_place_so101_battery_sort_9v_aa_330eps

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [zeuzei/pick_place_so101_battery_sort_9v_aa_330eps](https://huggingface.co/datasets/zeuzei/pick_place_so101_battery_sort_9v_aa_330eps), revision `6a33170c3844561a7150ff48c51c8af5d7170812` |
| Author | zeus ([zeuzei](https://huggingface.co/zeuzei)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 330 episodes, 212,337 frames |
| Rank | 70 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 785 | 793 |
| shoulder lift | 360/4,095 degree (degrees) | 1994 | 1995 |
| elbow flex | 360/4,095 degree (degrees) | 2277 | 2246 |
| wrist flex | 360/4,095 degree (degrees) | 1050 | 1072 |
| wrist roll | 360/4,095 degree (degrees) | 2093 | 2101 |
| gripper | 0.06333 of LeRobot's calibrated scale (a range of 1579 counts) | 887 | 748 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (98,713 frames); every frame then read as it would arrive. The held-out half (113,624 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 50 | 85 | 1 |
| shoulder lift | 58 | 142 | 4 |
| elbow flex | 58 | 173 | 11 |
| wrist flex | 42 | 115 | 0 |
| wrist roll | 17 | 148 | 55 |
| gripper | 16 | 118 | 3 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.347 | 0.312 | 0.689 | 0.515 |
| shoulder lift | 2.113 | 2.399 | 3.113 | 2.205 |
| elbow flex | 1.516 | 1.969 | 3.679 | 2.736 |
| wrist flex | 1.039 | 1.140 | 1.629 | 1.107 |
| wrist roll | 0.653 | 1.630 | 3.233 | 2.221 |
| gripper | 3.058 | 3.128 | 3.630 | 3.226 |
| five body joints | 1.295 | 1.654 | 2.713 | 1.937 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
