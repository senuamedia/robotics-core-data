# kulsum1/pick-cube-so101-simtwin

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [kulsum1/pick-cube-so101-simtwin](https://huggingface.co/datasets/kulsum1/pick-cube-so101-simtwin), revision `a36ce3d316b3c870d513a515ed2fad3051c27f7a` |
| Author | KULSUM JAHA MOHAMMED ([kulsum1](https://huggingface.co/kulsum1)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 195 episodes, 115,730 frames |
| Rank | 160 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2109 | 2163 |
| shoulder lift | 360/4,095 degree (degrees) | 2018 | 2027 |
| elbow flex | 360/4,095 degree (degrees) | 2068 | 2149 |
| wrist flex | 360/4,095 degree (degrees) | 1013 | 1108 |
| wrist roll | 360/4,095 degree (degrees) | 1815 | 1815 |
| gripper | 0.06570 of LeRobot's calibrated scale (a range of 1522 counts) | 1248 | 1232 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (62,276 frames); every frame then read as it would arrive. The held-out half (53,454 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 42 | 103 | 43 |
| shoulder lift | 58 | 126 | 112 |
| elbow flex | 56 | 116 | 157 |
| wrist flex | 43 | 107 | 188 |
| wrist roll | 24 | 136 | 210 |
| gripper | 17 | 119 | 27 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.350 | 0.910 | 2.194 | 1.429 |
| shoulder lift | 1.472 | 2.097 | 3.956 | 2.866 |
| elbow flex | 0.820 | 1.375 | 3.779 | 2.844 |
| wrist flex | 1.276 | 1.313 | 1.766 | 1.445 |
| wrist roll | 0.212 | 0.263 | 0.565 | 0.451 |
| gripper | 25.867 | 25.910 | 26.249 | 26.068 |
| five body joints | 0.962 | 1.335 | 2.763 | 2.032 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
