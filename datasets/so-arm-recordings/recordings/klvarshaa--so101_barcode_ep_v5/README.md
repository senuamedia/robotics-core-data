# klvarshaa/so101_barcode_ep_v5

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [klvarshaa/so101_barcode_ep_v5](https://huggingface.co/datasets/klvarshaa/so101_barcode_ep_v5), revision `5607c5369714d969382938d13e6d0c254208dd47` |
| Author | CLX ([klvarshaa](https://huggingface.co/klvarshaa)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 72 episodes, 206,435 frames |
| Rank | 75 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1831 | 1864 |
| shoulder lift | 360/4,095 degree (degrees) | 1846 | 1832 |
| elbow flex | 360/4,095 degree (degrees) | 1714 | 1754 |
| wrist flex | 360/4,095 degree (degrees) | 1822 | 1871 |
| wrist roll | 360/4,095 degree (degrees) | 2332 | 2333 |
| gripper | 0.06835 of LeRobot's calibrated scale (a range of 1463 counts) | 756 | 648 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (120,734 frames); every frame then read as it would arrive. The held-out half (85,701 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 66 | 92 | 0 |
| shoulder lift | 85 | 93 | 29 |
| elbow flex | 59 | 97 | 11 |
| wrist flex | 43 | 111 | 2 |
| wrist roll | 43 | 117 | 1 |
| gripper | 16 | 118 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.419 | 0.812 | 1.743 | 1.254 |
| shoulder lift | 1.196 | 1.630 | 3.019 | 2.283 |
| elbow flex | 0.967 | 1.183 | 3.273 | 2.598 |
| wrist flex | 0.481 | 0.989 | 2.374 | 1.582 |
| wrist roll | 0.459 | 0.769 | 1.785 | 1.192 |
| gripper | 2.308 | 2.387 | 2.823 | 2.448 |
| five body joints | 0.772 | 1.121 | 2.517 | 1.868 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
