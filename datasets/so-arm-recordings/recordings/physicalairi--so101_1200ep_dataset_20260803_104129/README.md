# physicalairi/so101_1200ep_dataset_20260803_104129

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [physicalairi/so101_1200ep_dataset_20260803_104129](https://huggingface.co/datasets/physicalairi/so101_1200ep_dataset_20260803_104129), revision `d7b78e5cc28687d033e16d92e35776b7da745d2e` |
| Author | フィジカルAI総合研究所 ([physicalairi](https://huggingface.co/physicalairi)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 650 episodes, 832,185 frames |
| Rank | 12 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1093 | 1110 |
| shoulder lift | 360/4,095 degree (degrees) | 2322 | 2335 |
| elbow flex | 360/4,095 degree (degrees) | 2012 | 2023 |
| wrist flex | 360/4,095 degree (degrees) | 2252 | 2270 |
| wrist roll | 360/4,095 degree (degrees) | 1841 | 1881 |
| gripper | 0.06353 of LeRobot's calibrated scale (a range of 1574 counts) | 1326 | 1084 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (427,090 frames); every frame then read as it would arrive. The held-out half (405,095 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 62 | 96 | 5 |
| shoulder lift | 116 | 149 | 15 |
| elbow flex | 113 | 144 | 23 |
| wrist flex | 101 | 154 | 56 |
| wrist roll | 96 | 164 | 59 |
| gripper | 47 | 120 | 7 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.457 | 0.827 | 1.815 | 1.295 |
| shoulder lift | 1.749 | 2.550 | 4.072 | 2.908 |
| elbow flex | 0.747 | 1.507 | 3.913 | 2.765 |
| wrist flex | 0.613 | 1.668 | 3.570 | 2.426 |
| wrist roll | 0.473 | 0.834 | 1.770 | 1.196 |
| gripper | 4.969 | 5.024 | 8.476 | 7.278 |
| five body joints | 0.940 | 1.608 | 3.195 | 2.240 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
