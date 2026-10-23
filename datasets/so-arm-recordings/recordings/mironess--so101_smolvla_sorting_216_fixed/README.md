# mironess/so101_smolvla_sorting_216_fixed

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [mironess/so101_smolvla_sorting_216_fixed](https://huggingface.co/datasets/mironess/so101_smolvla_sorting_216_fixed), revision `81da99995d936dec0edb73d3ae95696f86198ef4` |
| Author | [mironess](https://huggingface.co/mironess) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 216 episodes, 90,489 frames |
| Rank | 227 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1095 | 1114 |
| shoulder lift | 360/4,095 degree (degrees) | 2032 | 2054 |
| elbow flex | 360/4,095 degree (degrees) | 1957 | 2212 |
| wrist flex | 360/4,095 degree (degrees) | 804 | 830 |
| wrist roll | 360/4,095 degree (degrees) | 2332 | 2374 |
| gripper | 0.21231 of LeRobot's calibrated scale (a range of 471 counts) | 455 | 709 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (46,288 frames); every frame then read as it would arrive. The held-out half (44,201 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 40 | 89 | 2 |
| shoulder lift | 22 | 173 | 2 |
| elbow flex | 32 | 173 | 1 |
| wrist flex | 27 | 125 | 4 |
| wrist roll | 22 | 140 | 17 |
| gripper | 15 | 90 | 4 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.558 | 1.034 | 1.847 | 1.417 |
| shoulder lift | 1.821 | 4.315 | 6.676 | 5.254 |
| elbow flex | 4.519 | 6.117 | 8.632 | 7.381 |
| wrist flex | 0.957 | 2.006 | 3.427 | 2.620 |
| wrist roll | 0.653 | 1.303 | 2.327 | 1.757 |
| gripper | 6.361 | 6.639 | 7.362 | 6.943 |
| five body joints | 2.253 | 3.544 | 5.284 | 4.337 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
