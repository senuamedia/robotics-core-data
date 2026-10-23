# zeraaak/so101_blue_cube_black_box_100Demos

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [zeraaak/so101_blue_cube_black_box_100Demos](https://huggingface.co/datasets/zeraaak/so101_blue_cube_black_box_100Demos), revision `17753de59381dfb81fc8024297650d24b460d31c` |
| Author | Yamen Al Ahmad ([zeraaak](https://huggingface.co/zeraaak)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 252 episodes, 123,839 frames |
| Rank | 142 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07516 of LeRobot's calibrated scale (a range of 2661 counts) | 2009 | 2065 |
| shoulder lift | 0.08677 of LeRobot's calibrated scale (a range of 2305 counts) | 1684 | 1758 |
| elbow flex | 0.08921 of LeRobot's calibrated scale (a range of 2242 counts) | 1647 | 1664 |
| wrist flex | 0.08632 of LeRobot's calibrated scale (a range of 2317 counts) | 1075 | 1080 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1786 | 1796 |
| gripper | 0.08937 of LeRobot's calibrated scale (a range of 1119 counts) | 751 | 822 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (72,177 frames); every frame then read as it would arrive. The held-out half (51,662 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 32 | 148 | 8 |
| shoulder lift | 10 | 201 | 37 |
| elbow flex | 19 | 217 | 16 |
| wrist flex | 54 | 171 | 21 |
| wrist roll | 42 | 178 | 11 |
| gripper | 4 | 186 | 13 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.498 | 1.270 | 3.007 | 1.945 |
| shoulder lift | 1.227 | 2.329 | 4.782 | 3.220 |
| elbow flex | 1.400 | 2.323 | 4.745 | 3.244 |
| wrist flex | 1.177 | 1.533 | 3.128 | 1.984 |
| wrist roll | 0.538 | 1.292 | 3.203 | 2.067 |
| gripper | 5.598 | 5.399 | 6.390 | 5.788 |
| five body joints | 1.037 | 1.814 | 3.859 | 2.564 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
