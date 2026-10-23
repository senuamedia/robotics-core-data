# karthikpythireddi93/breakfast_sandiwch

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [karthikpythireddi93/breakfast_sandiwch](https://huggingface.co/datasets/karthikpythireddi93/breakfast_sandiwch), revision `c7f8ccdf2aa88cba84a279e59685dc49dab96c64` |
| Author | [karthikpythireddi93](https://huggingface.co/karthikpythireddi93) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 103 episodes, 84,738 frames |
| Rank | 253 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07342 of LeRobot's calibrated scale (a range of 2724 counts) | 1076 | 1098 |
| shoulder lift | 0.08295 of LeRobot's calibrated scale (a range of 2411 counts) | 1789 | 1754 |
| elbow flex | 0.09079 of LeRobot's calibrated scale (a range of 2203 counts) | 1238 | 1348 |
| wrist flex | 0.10368 of LeRobot's calibrated scale (a range of 1929 counts) | 1284 | 1186 |
| wrist roll | 0.05252 of LeRobot's calibrated scale (a range of 3808 counts) | 2350 | 2378 |
| gripper | 0.06752 of LeRobot's calibrated scale (a range of 1481 counts) | 836 | 716 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (48,586 frames); every frame then read as it would arrive. The held-out half (36,152 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 69 | 0 |
| shoulder lift | 65 | 93 | 2 |
| elbow flex | 52 | 74 | 6 |
| wrist flex | 30 | 101 | 70 |
| wrist roll | 22 | 122 | 38 |
| gripper | 14 | 114 | 4 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.239 | 0.456 | 1.342 | 0.878 |
| shoulder lift | 1.477 | 1.716 | 2.445 | 2.012 |
| elbow flex | 0.758 | 0.900 | 2.806 | 2.562 |
| wrist flex | 0.440 | 0.634 | 1.529 | 1.003 |
| wrist roll | 0.342 | 0.863 | 2.232 | 1.393 |
| gripper | 1.071 | 1.360 | 3.250 | 2.332 |
| five body joints | 0.790 | 1.010 | 2.143 | 1.692 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
