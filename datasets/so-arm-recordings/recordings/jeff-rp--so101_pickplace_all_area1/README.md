# jeff-rp/so101_pickplace_all_area1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [jeff-rp/so101_pickplace_all_area1](https://huggingface.co/datasets/jeff-rp/so101_pickplace_all_area1), revision `6ade7fa3611a48d19d1ccd465e0cea06308255c6` |
| Author | Jeff Lin ([jeff-rp](https://huggingface.co/jeff-rp)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 230 episodes, 184,036 frames |
| Rank | 90 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 737 | 748 |
| shoulder lift | 360/4,095 degree (degrees) | 1701 | 1693 |
| elbow flex | 360/4,095 degree (degrees) | 1766 | 1810 |
| wrist flex | 360/4,095 degree (degrees) | 1981 | 2011 |
| wrist roll | 360/4,095 degree (degrees) | 1984 | 1996 |
| gripper | 0.06592 of LeRobot's calibrated scale (a range of 1517 counts) | 524 | 412 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (89,687 frames); every frame then read as it would arrive. The held-out half (94,349 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 43 | 47 | 0 |
| shoulder lift | 46 | 55 | 11 |
| elbow flex | 52 | 68 | 0 |
| wrist flex | 53 | 69 | 16 |
| wrist roll | 45 | 65 | 7 |
| gripper | 21 | 97 | 3 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.181 | 0.208 | 0.619 | 0.435 |
| shoulder lift | 2.251 | 2.432 | 2.275 | 1.838 |
| elbow flex | 1.208 | 1.324 | 2.990 | 2.572 |
| wrist flex | 0.242 | 0.293 | 0.817 | 0.558 |
| wrist roll | 0.273 | 0.468 | 1.281 | 0.827 |
| gripper | 7.122 | 7.139 | 7.236 | 7.128 |
| five body joints | 1.156 | 1.266 | 1.833 | 1.495 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
