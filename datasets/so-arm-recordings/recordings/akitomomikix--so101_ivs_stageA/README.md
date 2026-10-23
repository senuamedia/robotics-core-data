# akitomomikix/so101_ivs_stageA

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [akitomomikix/so101_ivs_stageA](https://huggingface.co/datasets/akitomomikix/so101_ivs_stageA), revision `d59837859e1c7321f631675cd6a261be1564a751` |
| Author | [akitomomikix](https://huggingface.co/akitomomikix) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 150 episodes, 87,117 frames |
| Rank | 244 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 979 | 985 |
| shoulder lift | 360/4,095 degree (degrees) | 1729 | 1817 |
| elbow flex | 360/4,095 degree (degrees) | 1808 | 1851 |
| wrist flex | 360/4,095 degree (degrees) | 603 | 646 |
| wrist roll | 360/4,095 degree (degrees) | 1523 | 1547 |
| gripper | 0.06671 of LeRobot's calibrated scale (a range of 1499 counts) | 942 | 807 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (43,837 frames); every frame then read as it would arrive. The held-out half (43,280 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 36 | 124 | 1 |
| shoulder lift | 33 | 161 | 18 |
| elbow flex | 39 | 154 | 18 |
| wrist flex | 26 | 125 | 6 |
| wrist roll | 43 | 141 | 3 |
| gripper | 18 | 179 | 15 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.226 | 0.288 | 0.832 | 0.572 |
| shoulder lift | 2.678 | 2.913 | 3.714 | 3.241 |
| elbow flex | 1.157 | 1.353 | 2.907 | 2.288 |
| wrist flex | 1.759 | 1.844 | 1.749 | 1.325 |
| wrist roll | 0.304 | 0.437 | 1.107 | 0.787 |
| gripper | 9.094 | 9.119 | 9.170 | 9.057 |
| five body joints | 1.532 | 1.672 | 2.333 | 1.920 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
