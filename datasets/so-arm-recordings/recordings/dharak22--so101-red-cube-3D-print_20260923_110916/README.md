# dharak22/so101-red-cube-3D-print_20260923_110916

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [dharak22/so101-red-cube-3D-print_20260923_110916](https://huggingface.co/datasets/dharak22/so101-red-cube-3D-print_20260923_110916), revision `98cb9bdf6201bd204fa08c8e267244c4085fe5da` |
| Author | Solanki Dharak Deepak ([dharak22](https://huggingface.co/dharak22)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 198 episodes, 117,795 frames |
| Rank | 154 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2145 | 2195 |
| shoulder lift | 360/4,095 degree (degrees) | 2018 | 2027 |
| elbow flex | 360/4,095 degree (degrees) | 2068 | 2149 |
| wrist flex | 360/4,095 degree (degrees) | 1013 | 1109 |
| wrist roll | 360/4,095 degree (degrees) | 1816 | 1816 |
| gripper | 0.06570 of LeRobot's calibrated scale (a range of 1522 counts) | 1248 | 1232 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (63,357 frames); every frame then read as it would arrive. The held-out half (54,520 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 45 | 104 | 43 |
| shoulder lift | 57 | 126 | 112 |
| elbow flex | 59 | 116 | 157 |
| wrist flex | 35 | 108 | 188 |
| wrist roll | 44 | 138 | 210 |
| gripper | 26 | 119 | 27 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.398 | 0.530 | 1.114 | 0.821 |
| shoulder lift | 2.040 | 2.531 | 4.197 | 3.249 |
| elbow flex | 0.811 | 1.464 | 3.642 | 2.801 |
| wrist flex | 2.294 | 2.402 | 2.797 | 2.450 |
| wrist roll | 0.302 | 0.448 | 0.975 | 0.669 |
| gripper | 17.936 | 17.966 | 18.246 | 18.126 |
| five body joints | 1.437 | 1.720 | 2.859 | 2.259 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
