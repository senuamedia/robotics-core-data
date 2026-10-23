# kvnptl/so101-teleop-vials-to-rack-real

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [kvnptl/so101-teleop-vials-to-rack-real](https://huggingface.co/datasets/kvnptl/so101-teleop-vials-to-rack-real), revision `b9f73d7e9009e2205bf6db464903b735fb43c27a` |
| Author | [kvnptl](https://huggingface.co/kvnptl) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 106,002 frames |
| Rank | 180 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07391 of LeRobot's calibrated scale (a range of 2706 counts) | 1056 | 1054 |
| shoulder lift | 0.08482 of LeRobot's calibrated scale (a range of 2358 counts) | 1974 | 1981 |
| elbow flex | 0.09095 of LeRobot's calibrated scale (a range of 2199 counts) | 2016 | 2054 |
| wrist flex | 0.08730 of LeRobot's calibrated scale (a range of 2291 counts) | 2127 | 2161 |
| wrist roll | 0.05199 of LeRobot's calibrated scale (a range of 3847 counts) | 2085 | 2122 |
| gripper | 0.06676 of LeRobot's calibrated scale (a range of 1498 counts) | 705 | 588 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (42,818 frames); every frame then read as it would arrive. The held-out half (63,184 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 26 | 44 | 115 |
| shoulder lift | 54 | 75 | 2 |
| elbow flex | 55 | 68 | 38 |
| wrist flex | 42 | 91 | 19 |
| wrist roll | 41 | 59 | 64 |
| gripper | 25 | 106 | 48 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 22.2 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.195 | 0.197 | 0.698 | 0.476 |
| shoulder lift | 1.356 | 1.618 | 2.946 | 2.294 |
| elbow flex | 1.165 | 1.414 | 2.759 | 2.177 |
| wrist flex | 0.428 | 0.581 | 1.639 | 1.073 |
| wrist roll | 0.231 | 0.312 | 1.127 | 0.669 |
| gripper | 8.268 | 8.274 | 8.630 | 8.380 |
| five body joints | 0.833 | 1.008 | 2.036 | 1.538 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
