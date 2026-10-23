# msmandelbrot/so101_pick_and_place_green_cube_black_box

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [msmandelbrot/so101_pick_and_place_green_cube_black_box](https://huggingface.co/datasets/msmandelbrot/so101_pick_and_place_green_cube_black_box), revision `0a93549e1675e9c5f8fa1a95fa54481582d4f09f` |
| Author | Anastasiia Romanenko ([msmandelbrot](https://huggingface.co/msmandelbrot)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 121,990 frames |
| Rank | 144 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07291 of LeRobot's calibrated scale (a range of 2743 counts) | 1562 | 1587 |
| shoulder lift | 0.08340 of LeRobot's calibrated scale (a range of 2398 counts) | 2144 | 2156 |
| elbow flex | 0.08945 of LeRobot's calibrated scale (a range of 2236 counts) | 2160 | 2199 |
| wrist flex | 0.08432 of LeRobot's calibrated scale (a range of 2372 counts) | 2121 | 2120 |
| wrist roll | 0.10482 of LeRobot's calibrated scale (a range of 1908 counts) | 1310 | 1311 |
| gripper | 0.06575 of LeRobot's calibrated scale (a range of 1521 counts) | 921 | 824 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (63,299 frames); every frame then read as it would arrive. The held-out half (58,691 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 63 | 88 | 5 |
| shoulder lift | 52 | 101 | 7 |
| elbow flex | 57 | 105 | 1 |
| wrist flex | 65 | 107 | 6 |
| wrist roll | 38 | 93 | 8 |
| gripper | 25 | 116 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.502 | 0.784 | 1.698 | 1.190 |
| shoulder lift | 1.843 | 2.197 | 3.399 | 2.731 |
| elbow flex | 1.471 | 1.839 | 3.367 | 2.838 |
| wrist flex | 1.647 | 1.727 | 1.584 | 1.167 |
| wrist roll | 0.555 | 0.747 | 1.342 | 1.069 |
| gripper | 5.167 | 5.222 | 5.534 | 5.298 |
| five body joints | 1.328 | 1.572 | 2.453 | 1.971 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
