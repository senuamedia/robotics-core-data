# Ayyoub2026/so101_box_to_paper

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Ayyoub2026/so101_box_to_paper](https://huggingface.co/datasets/Ayyoub2026/so101_box_to_paper), revision `c23b81ec85f36ea2ff726514358b4ce49e0284ea` |
| Author | [Ayyoub2026](https://huggingface.co/Ayyoub2026) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 46 episodes, 82,593 frames |
| Rank | 259 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 774 | 791 |
| shoulder lift | 360/4,095 degree (degrees) | 1825 | 1821 |
| elbow flex | 360/4,095 degree (degrees) | 1567 | 1591 |
| wrist flex | 360/4,095 degree (degrees) | 594 | 602 |
| wrist roll | 360/4,095 degree (degrees) | 484 | 507 |
| gripper | 0.06803 of LeRobot's calibrated scale (a range of 1470 counts) | 1393 | 1176 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (41,297 frames); every frame then read as it would arrive. The held-out half (41,296 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 20 | 23 | 5 |
| shoulder lift | 34 | 40 | 0 |
| elbow flex | 19 | 38 | 7 |
| wrist flex | 19 | 40 | 14 |
| wrist roll | 20 | 43 | 20 |
| gripper | 11 | 118 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.181 | 0.166 | 0.362 | 0.296 |
| shoulder lift | 1.869 | 1.949 | 0.738 | 0.522 |
| elbow flex | 1.197 | 1.230 | 3.019 | 2.796 |
| wrist flex | 0.306 | 0.302 | 0.646 | 0.612 |
| wrist roll | 0.236 | 0.273 | 0.721 | 0.531 |
| gripper | 2.358 | 2.507 | 3.797 | 3.153 |
| five body joints | 1.010 | 1.049 | 1.464 | 1.328 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
