# jinseonylee/SO101_PickAndPlace_Fruit

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [jinseonylee/SO101_PickAndPlace_Fruit](https://huggingface.co/datasets/jinseonylee/SO101_PickAndPlace_Fruit), revision `13151b3dd9cf03c1139e61594e54f7a7e3a302d2` |
| Author | [jinseonylee](https://huggingface.co/jinseonylee) |
| Licence | mit (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 102 episodes, 78,111 frames |
| Rank | 276 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.11614 of LeRobot's calibrated scale (a range of 1722 counts) | 1168 | 1330 |
| shoulder lift | 0.09149 of LeRobot's calibrated scale (a range of 2186 counts) | 1942 | 1939 |
| elbow flex | 0.09488 of LeRobot's calibrated scale (a range of 2108 counts) | 2108 | 2196 |
| wrist flex | 0.09050 of LeRobot's calibrated scale (a range of 2210 counts) | 1441 | 1497 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 2781 | 2767 |
| gripper | 0.06978 of LeRobot's calibrated scale (a range of 1433 counts) | 1087 | 916 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (40,397 frames); every frame then read as it would arrive. The held-out half (37,714 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 37 | 73 | 4 |
| shoulder lift | 46 | 115 | 22 |
| elbow flex | 42 | 145 | 41 |
| wrist flex | 41 | 136 | 38 |
| wrist roll | 28 | 124 | 20 |
| gripper | 17 | 130 | 2 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.614 | 0.815 | 1.520 | 1.105 |
| shoulder lift | 3.314 | 3.675 | 2.989 | 2.541 |
| elbow flex | 2.952 | 4.094 | 9.470 | 8.563 |
| wrist flex | 2.251 | 3.484 | 8.217 | 7.331 |
| wrist roll | 0.929 | 1.757 | 3.511 | 2.434 |
| gripper | 5.429 | 5.667 | 8.394 | 7.136 |
| five body joints | 2.280 | 3.038 | 6.012 | 5.304 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under mit; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
