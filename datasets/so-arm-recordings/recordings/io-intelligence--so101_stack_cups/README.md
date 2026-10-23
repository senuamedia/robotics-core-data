# io-intelligence/so101_stack_cups

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [io-intelligence/so101_stack_cups](https://huggingface.co/datasets/io-intelligence/so101_stack_cups), revision `ad5f7586c8501430754d3e88bdd6b5d7dce07999` |
| Author | IO Intelligence ([io-intelligence](https://huggingface.co/io-intelligence)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 660 episodes, 188,035 frames |
| Rank | 86 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1419 | 1435 |
| shoulder lift | 360/4,095 degree (degrees) | 1880 | 1903 |
| elbow flex | 360/4,095 degree (degrees) | 2060 | 2104 |
| wrist flex | 360/4,095 degree (degrees) | 766 | 791 |
| wrist roll | 360/4,095 degree (degrees) | 1337 | 1358 |
| gripper | 0.06515 of LeRobot's calibrated scale (a range of 1535 counts) | 836 | 738 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (102,626 frames); every frame then read as it would arrive. The held-out half (85,409 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 80 | 128 | 15 |
| shoulder lift | 68 | 170 | 7 |
| elbow flex | 50 | 175 | 10 |
| wrist flex | 60 | 129 | 67 |
| wrist roll | 53 | 85 | 414 |
| gripper | 30 | 135 | 4 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 3 sample episodes from the held-out half, 597 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.551 | 1.097 | 2.816 | 1.794 |
| shoulder lift | 1.252 | 2.626 | 5.601 | 3.767 |
| elbow flex | 1.513 | 2.564 | 5.515 | 3.795 |
| wrist flex | 1.257 | 1.583 | 3.125 | 2.020 |
| wrist roll | 0.492 | 0.706 | 1.702 | 1.150 |
| gripper | 1.139 | 1.514 | 4.897 | 3.633 |
| five body joints | 1.094 | 1.879 | 4.058 | 2.728 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
