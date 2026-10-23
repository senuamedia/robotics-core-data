# Stands99/so101_gather_dolls_260923_120ep_20260923_163400

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Stands99/so101_gather_dolls_260923_120ep_20260923_163400](https://huggingface.co/datasets/Stands99/so101_gather_dolls_260923_120ep_20260923_163400), revision `d1503e96f508de636a62b35958f56e3dcde5d808` |
| Author | [Stands99](https://huggingface.co/Stands99) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 25 frames a second |
| Recording | 120 episodes, 108,657 frames |
| Rank | 176 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1780 | 1775 |
| shoulder lift | 360/4,095 degree (degrees) | 1959 | 2005 |
| elbow flex | 360/4,095 degree (degrees) | 2169 | 2196 |
| wrist flex | 360/4,095 degree (degrees) | 922 | 932 |
| wrist roll | 360/4,095 degree (degrees) | 1870 | 1881 |
| gripper | 0.06702 of LeRobot's calibrated scale (a range of 1492 counts) | 779 | 707 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (51,847 frames); every frame then read as it would arrive. The held-out half (56,810 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 44 | 132 | 0 |
| shoulder lift | 54 | 137 | 1 |
| elbow flex | 55 | 142 | 3 |
| wrist flex | 44 | 134 | 0 |
| wrist roll | 48 | 140 | 0 |
| gripper | 8 | 163 | 21 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 499 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 34.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.419 | 0.496 | 1.550 | 0.733 |
| shoulder lift | 1.804 | 2.061 | 2.572 | 2.000 |
| elbow flex | 1.263 | 1.522 | 3.255 | 2.582 |
| wrist flex | 3.955 | 3.958 | 1.284 | 0.654 |
| wrist roll | 0.544 | 0.268 | 1.045 | 0.442 |
| gripper | 3.269 | 3.419 | 4.108 | 3.547 |
| five body joints | 2.047 | 2.123 | 2.114 | 1.538 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
