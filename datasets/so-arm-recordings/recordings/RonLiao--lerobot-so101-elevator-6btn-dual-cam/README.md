# RonLiao/lerobot-so101-elevator-6btn-dual-cam

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [RonLiao/lerobot-so101-elevator-6btn-dual-cam](https://huggingface.co/datasets/RonLiao/lerobot-so101-elevator-6btn-dual-cam), revision `fec00ee4e3290a4b3de2191efed773cb06125056` |
| Author | [RonLiao](https://huggingface.co/RonLiao) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 600 episodes, 144,494 frames |
| Rank | 120 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07212 of LeRobot's calibrated scale (a range of 2773 counts) | 1243 | 1262 |
| shoulder lift | 0.08730 of LeRobot's calibrated scale (a range of 2291 counts) | 1195 | 1304 |
| elbow flex | 0.08933 of LeRobot's calibrated scale (a range of 2239 counts) | 1659 | 1678 |
| wrist flex | 0.08692 of LeRobot's calibrated scale (a range of 2301 counts) | 2147 | 2206 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1695 | 1689 |
| gripper | 0.06676 of LeRobot's calibrated scale (a range of 1498 counts) | 17 | 16 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (72,248 frames); every frame then read as it would arrive. The held-out half (72,246 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 70 | 187 | 17 |
| shoulder lift | 68 | 153 | 40 |
| elbow flex | 44 | 202 | 2 |
| wrist flex | 38 | 297 | 46 |
| wrist roll | 239 | 282 | 0 |
| gripper | 14 | 14 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 1.503 | 1.818 | 2.934 | 2.289 |
| shoulder lift | 2.232 | 2.721 | 3.553 | 2.517 |
| elbow flex | 0.964 | 1.858 | 4.195 | 3.198 |
| wrist flex | 2.002 | 4.786 | 10.014 | 8.326 |
| wrist roll | 0.139 | 0.140 | 0.411 | 0.399 |
| gripper | 0.245 | 0.245 | 0.219 | 0.218 |
| five body joints | 1.561 | 2.723 | 5.277 | 4.272 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
