# samanthalhy/so100_popcorn_1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [samanthalhy/so100_popcorn_1](https://huggingface.co/datasets/samanthalhy/so100_popcorn_1), revision `feb9f146a8af68fc030d49804793940eddaa408f` |
| Author | Samantha Lee ([samanthalhy](https://huggingface.co/samanthalhy)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so100_follower` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 202 episodes, 367,883 frames |
| Rank | 34 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09298 of LeRobot's calibrated scale (a range of 2151 counts) | 1061 | 1114 |
| shoulder lift | 0.08485 of LeRobot's calibrated scale (a range of 2357 counts) | 1418 | 1430 |
| elbow flex | 0.09091 of LeRobot's calibrated scale (a range of 2200 counts) | 1715 | 1760 |
| wrist flex | 0.08666 of LeRobot's calibrated scale (a range of 2308 counts) | 537 | 558 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 2403 | 2458 |
| gripper | 0.07138 of LeRobot's calibrated scale (a range of 1401 counts) | 7 | 13 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (177,156 frames); every frame then read as it would arrive. The held-out half (190,727 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 56 | 139 | 24 |
| shoulder lift | 43 | 156 | 20 |
| elbow flex | 49 | 178 | 33 |
| wrist flex | 72 | 151 | 7 |
| wrist roll | 70 | 213 | 115 |
| gripper | 4 | 7 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.528 | 1.225 | 2.285 | 1.724 |
| shoulder lift | 1.798 | 1.999 | 2.409 | 2.217 |
| elbow flex | 0.918 | 1.309 | 2.774 | 2.349 |
| wrist flex | 2.344 | 2.410 | 1.584 | 1.229 |
| wrist roll | 0.696 | 2.361 | 4.296 | 3.091 |
| gripper | 2.801 | 2.805 | 3.083 | 3.080 |
| five body joints | 1.437 | 1.928 | 2.816 | 2.212 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
