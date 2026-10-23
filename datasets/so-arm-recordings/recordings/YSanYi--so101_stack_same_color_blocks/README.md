# YSanYi/so101_stack_same_color_blocks

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [YSanYi/so101_stack_same_color_blocks](https://huggingface.co/datasets/YSanYi/so101_stack_same_color_blocks), revision `3e9f0775febe5d7295af8efc23dc09e5a4de3b30` |
| Author | [YSanYi](https://huggingface.co/YSanYi) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 122 episodes, 81,056 frames |
| Rank | 264 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07291 of LeRobot's calibrated scale (a range of 2743 counts) | 890 | 902 |
| shoulder lift | 0.08313 of LeRobot's calibrated scale (a range of 2406 counts) | 1982 | 1973 |
| elbow flex | 0.09046 of LeRobot's calibrated scale (a range of 2211 counts) | 1922 | 1938 |
| wrist flex | 0.08442 of LeRobot's calibrated scale (a range of 2369 counts) | 837 | 834 |
| wrist roll | 0.05219 of LeRobot's calibrated scale (a range of 3832 counts) | 1044 | 1079 |
| gripper | 0.06707 of LeRobot's calibrated scale (a range of 1491 counts) | 1071 | 958 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (43,211 frames); every frame then read as it would arrive. The held-out half (39,331 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 297 | 475 | 548 |
| shoulder lift | 485 | 819 | 360 |
| elbow flex | 490 | 757 | 681 |
| wrist flex | 225 | 378 | 286 |
| wrist roll | 290 | 424 | 237 |
| gripper | 264 | 473 | 174 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.264 | 0.329 | 0.966 | 0.652 |
| shoulder lift | 1.834 | 2.370 | 3.677 | 2.638 |
| elbow flex | 0.776 | 1.455 | 3.248 | 2.388 |
| wrist flex | 1.917 | 1.945 | 1.320 | 0.866 |
| wrist roll | 0.373 | 0.465 | 1.254 | 0.827 |
| gripper | 2.124 | 2.718 | 8.120 | 6.878 |
| five body joints | 1.253 | 1.538 | 2.380 | 1.704 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
