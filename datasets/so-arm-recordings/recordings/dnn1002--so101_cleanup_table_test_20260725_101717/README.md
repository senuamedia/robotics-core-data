# dnn1002/so101_cleanup_table_test_20260725_101717

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [dnn1002/so101_cleanup_table_test_20260725_101717](https://huggingface.co/datasets/dnn1002/so101_cleanup_table_test_20260725_101717), revision `6093f305be4734703f53ac6cad524a0c19a72082` |
| Author | Nhat Nam Duong ([dnn1002](https://huggingface.co/dnn1002)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 401 episodes, 517,249 frames |
| Rank | 22 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1887 | 1916 |
| shoulder lift | 360/4,095 degree (degrees) | 2035 | 2057 |
| elbow flex | 360/4,095 degree (degrees) | 1899 | 1958 |
| wrist flex | 360/4,095 degree (degrees) | 974 | 1016 |
| wrist roll | 360/4,095 degree (degrees) | 2205 | 2297 |
| gripper | 0.06631 of LeRobot's calibrated scale (a range of 1508 counts) | 970 | 799 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (303,803 frames); every frame then read as it would arrive. The held-out half (213,446 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 71 | 79 | 52 |
| shoulder lift | 77 | 89 | 12 |
| elbow flex | 75 | 91 | 4 |
| wrist flex | 70 | 76 | 2 |
| wrist roll | 98 | 104 | 46 |
| gripper | 44 | 122 | 40 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.292 | 0.689 | 1.694 | 1.122 |
| shoulder lift | 1.110 | 1.260 | 1.695 | 1.432 |
| elbow flex | 0.958 | 1.034 | 3.068 | 2.874 |
| wrist flex | 0.300 | 0.300 | 0.690 | 0.529 |
| wrist roll | 0.367 | 0.624 | 1.531 | 0.972 |
| gripper | 3.084 | 3.370 | 4.680 | 3.827 |
| five body joints | 0.701 | 0.849 | 1.895 | 1.599 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
