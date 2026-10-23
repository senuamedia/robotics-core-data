# jackdzk/pick_place_charger_soarm101-2cam

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [jackdzk/pick_place_charger_soarm101-2cam](https://huggingface.co/datasets/jackdzk/pick_place_charger_soarm101-2cam), revision `48a5410dda5b640f53f026212ae52c127ccc5aec` |
| Author | [jackdzk](https://huggingface.co/jackdzk) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 175 episodes, 104,940 frames |
| Rank | 185 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1373 | 1381 |
| shoulder lift | 360/4,095 degree (degrees) | 1968 | 1976 |
| elbow flex | 360/4,095 degree (degrees) | 2179 | 2206 |
| wrist flex | 360/4,095 degree (degrees) | 762 | 769 |
| wrist roll | 360/4,095 degree (degrees) | 1248 | 1258 |
| gripper | 0.06485 of LeRobot's calibrated scale (a range of 1542 counts) | 773 | 608 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (52,185 frames); every frame then read as it would arrive. The held-out half (52,755 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 33 | 121 | 7 |
| shoulder lift | 18 | 154 | 20 |
| elbow flex | 22 | 160 | 18 |
| wrist flex | 23 | 103 | 7 |
| wrist roll | 16 | 66 | 127 |
| gripper | 12 | 115 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.370 | 1.159 | 2.530 | 1.681 |
| shoulder lift | 1.351 | 2.044 | 3.651 | 2.472 |
| elbow flex | 1.671 | 2.350 | 3.950 | 2.830 |
| wrist flex | 1.943 | 1.959 | 1.453 | 1.085 |
| wrist roll | 0.247 | 0.219 | 0.578 | 0.480 |
| gripper | 6.601 | 6.608 | 8.072 | 7.483 |
| five body joints | 1.310 | 1.727 | 2.749 | 1.916 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
