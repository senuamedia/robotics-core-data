# RebelDot/single_so101_cubes

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [RebelDot/single_so101_cubes](https://huggingface.co/datasets/RebelDot/single_so101_cubes), revision `69ceb8def2aeb1ffc8a53b37fc23e1f635d9e60d` |
| Author | [RebelDot](https://huggingface.co/RebelDot) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 25 frames a second |
| Recording | 200 episodes, 88,772 frames |
| Rank | 238 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1163 | 1189 |
| shoulder lift | 360/4,095 degree (degrees) | 2109 | 2240 |
| elbow flex | 360/4,095 degree (degrees) | 1957 | 2006 |
| wrist flex | 360/4,095 degree (degrees) | 1396 | 1409 |
| wrist roll | 360/4,095 degree (degrees) | 1176 | 1215 |
| gripper | 0.06803 of LeRobot's calibrated scale (a range of 1470 counts) | 753 | 688 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (48,336 frames); every frame then read as it would arrive. The held-out half (40,436 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 58 | 162 | 1 |
| shoulder lift | 48 | 211 | 4 |
| elbow flex | 60 | 214 | 1 |
| wrist flex | 53 | 156 | 14 |
| wrist roll | 32 | 134 | 11 |
| gripper | 19 | 146 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 498 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 34.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.699 | 1.905 | 3.991 | 2.300 |
| shoulder lift | 3.883 | 4.541 | 6.107 | 4.655 |
| elbow flex | 1.509 | 2.565 | 4.818 | 3.134 |
| wrist flex | 0.775 | 1.487 | 3.251 | 1.868 |
| wrist roll | 0.450 | 0.694 | 1.583 | 0.950 |
| gripper | 2.640 | 2.730 | 5.626 | 4.203 |
| five body joints | 1.930 | 2.589 | 4.231 | 2.869 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
