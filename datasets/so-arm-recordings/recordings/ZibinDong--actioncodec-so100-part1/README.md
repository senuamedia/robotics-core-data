# ZibinDong/actioncodec-so100-part1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [ZibinDong/actioncodec-so100-part1](https://huggingface.co/datasets/ZibinDong/actioncodec-so100-part1), revision `f8e583a519d36f6054e4e120f8754faaabbbd465` |
| Author | Zibin Dong ([ZibinDong](https://huggingface.co/ZibinDong)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 500 episodes, 263,342 frames |
| Rank | 48 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07186 of LeRobot's calibrated scale (a range of 2783 counts) | 1429 | 978 |
| shoulder lift | 0.08613 of LeRobot's calibrated scale (a range of 2322 counts) | 1798 | 1797 |
| elbow flex | 0.09046 of LeRobot's calibrated scale (a range of 2211 counts) | 1746 | 1781 |
| wrist flex | 0.08261 of LeRobot's calibrated scale (a range of 2421 counts) | 975 | 987 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 3261 | 3336 |
| gripper | 0.06649 of LeRobot's calibrated scale (a range of 1504 counts) | 749 | 701 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (140,411 frames); every frame then read as it would arrive. The held-out half (122,931 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 81 | 266 | 3 |
| shoulder lift | 59 | 287 | 34 |
| elbow flex | 49 | 285 | 17 |
| wrist flex | 59 | 254 | 2 |
| wrist roll | 54 | 334 | 24 |
| gripper | 27 | 272 | 7 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.291 | 0.554 | 1.329 | 0.905 |
| shoulder lift | 1.342 | 1.855 | 2.649 | 1.965 |
| elbow flex | 0.557 | 0.956 | 3.605 | 3.132 |
| wrist flex | 0.316 | 0.655 | 1.472 | 1.013 |
| wrist roll | 0.367 | 1.825 | 3.762 | 2.534 |
| gripper | 7.606 | 7.635 | 7.712 | 7.601 |
| five body joints | 0.696 | 1.298 | 2.760 | 2.094 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
