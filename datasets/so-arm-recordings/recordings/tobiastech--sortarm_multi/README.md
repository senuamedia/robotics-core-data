# tobiastech/sortarm_multi

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [tobiastech/sortarm_multi](https://huggingface.co/datasets/tobiastech/sortarm_multi), revision `50c52636319e64da5e2f171de614aeee83c27d49` |
| Author | [tobiastech](https://huggingface.co/tobiastech) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 80 episodes, 105,246 frames |
| Rank | 182 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2254 | 2270 |
| shoulder lift | 360/4,095 degree (degrees) | 1598 | 1613 |
| elbow flex | 360/4,095 degree (degrees) | 1688 | 1734 |
| wrist flex | 360/4,095 degree (degrees) | 1139 | 1158 |
| wrist roll | 360/4,095 degree (degrees) | 716 | 740 |
| gripper | 0.06817 of LeRobot's calibrated scale (a range of 1467 counts) | 886 | 762 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (53,954 frames); every frame then read as it would arrive. The held-out half (51,292 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 59 | 83 | 0 |
| shoulder lift | 67 | 82 | 0 |
| elbow flex | 41 | 69 | 0 |
| wrist flex | 68 | 92 | 8 |
| wrist roll | 21 | 58 | 2 |
| gripper | 33 | 103 | 24 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.257 | 0.631 | 1.861 | 1.152 |
| shoulder lift | 1.021 | 1.356 | 2.579 | 1.913 |
| elbow flex | 0.827 | 1.059 | 3.034 | 2.507 |
| wrist flex | 2.359 | 2.421 | 2.245 | 1.676 |
| wrist roll | 0.300 | 0.285 | 0.739 | 0.491 |
| gripper | 2.061 | 2.197 | 3.025 | 2.423 |
| five body joints | 1.220 | 1.364 | 2.232 | 1.692 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
