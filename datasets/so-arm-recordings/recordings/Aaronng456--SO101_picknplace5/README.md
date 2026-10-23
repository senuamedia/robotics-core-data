# Aaronng456/SO101_picknplace5

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Aaronng456/SO101_picknplace5](https://huggingface.co/datasets/Aaronng456/SO101_picknplace5), revision `b5dad226705006ca6e14b4f0b723339b07b6fd26` |
| Author | [Aaronng456](https://huggingface.co/Aaronng456) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 101 episodes, 91,548 frames |
| Rank | 222 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07402 of LeRobot's calibrated scale (a range of 2702 counts) | 488 | 495 |
| shoulder lift | 0.08485 of LeRobot's calibrated scale (a range of 2357 counts) | 1953 | 1976 |
| elbow flex | 0.09029 of LeRobot's calibrated scale (a range of 2215 counts) | 1918 | 1998 |
| wrist flex | 0.08507 of LeRobot's calibrated scale (a range of 2351 counts) | 824 | 813 |
| wrist roll | 0.05203 of LeRobot's calibrated scale (a range of 3844 counts) | 402 | 416 |
| gripper | 0.06481 of LeRobot's calibrated scale (a range of 1543 counts) | 531 | 531 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (42,795 frames); every frame then read as it would arrive. The held-out half (48,753 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 35 | 49 | 20 |
| shoulder lift | 75 | 96 | 18 |
| elbow flex | 51 | 114 | 16 |
| wrist flex | 42 | 74 | 12 |
| wrist roll | 18 | 42 | 20 |
| gripper | 42 | 119 | 25 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.232 | 0.294 | 0.812 | 0.626 |
| shoulder lift | 1.998 | 2.361 | 2.456 | 2.225 |
| elbow flex | 0.908 | 1.290 | 3.900 | 3.568 |
| wrist flex | 0.371 | 0.519 | 1.268 | 0.946 |
| wrist roll | 0.303 | 0.268 | 0.635 | 0.495 |
| gripper | 1.317 | 1.455 | 1.937 | 1.669 |
| five body joints | 1.009 | 1.238 | 2.186 | 1.959 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
