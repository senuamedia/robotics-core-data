# tttonyalpha/so101_pick_cube_300_task3

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [tttonyalpha/so101_pick_cube_300_task3](https://huggingface.co/datasets/tttonyalpha/so101_pick_cube_300_task3), revision `d22485100688ef9d0ebc6a33013be61e2d5ecae2` |
| Author | Nikita ([tttonyalpha](https://huggingface.co/tttonyalpha)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 300 episodes, 116,016 frames |
| Rank | 157 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07463 of LeRobot's calibrated scale (a range of 2680 counts) | 1241 | 1289 |
| shoulder lift | 0.08190 of LeRobot's calibrated scale (a range of 2442 counts) | 2051 | 2114 |
| elbow flex | 0.08925 of LeRobot's calibrated scale (a range of 2241 counts) | 1695 | 1760 |
| wrist flex | 0.08114 of LeRobot's calibrated scale (a range of 2465 counts) | 1989 | 1815 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1803 | 1747 |
| gripper | 0.06341 of LeRobot's calibrated scale (a range of 1577 counts) | 1002 | 914 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (64,229 frames); every frame then read as it would arrive. The held-out half (51,787 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 38 | 91 | 13 |
| shoulder lift | 42 | 159 | 24 |
| elbow flex | 60 | 140 | 57 |
| wrist flex | 69 | 196 | 2 |
| wrist roll | 43 | 177 | 8 |
| gripper | 20 | 181 | 1 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.233 | 0.391 | 0.928 | 0.660 |
| shoulder lift | 1.836 | 3.103 | 4.931 | 3.658 |
| elbow flex | 0.934 | 1.974 | 3.522 | 2.703 |
| wrist flex | 1.014 | 2.631 | 4.631 | 3.382 |
| wrist roll | 0.370 | 0.719 | 1.437 | 1.050 |
| gripper | 9.637 | 10.012 | 11.363 | 10.879 |
| five body joints | 1.045 | 2.054 | 3.495 | 2.594 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
