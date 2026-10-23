# ssafy-d101/pick_left_a1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [ssafy-d101/pick_left_a1](https://huggingface.co/datasets/ssafy-d101/pick_left_a1), revision `ca15635a88ba5f1ebaee88aaae86b7a86ce2dec1` |
| Author | jabis ([ssafy-d101](https://huggingface.co/ssafy-d101)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 352 episodes, 198,630 frames |
| Rank | 78 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1696 | 1715 |
| shoulder lift | 360/4,095 degree (degrees) | 2359 | 2371 |
| elbow flex | 360/4,095 degree (degrees) | 2203 | 2218 |
| wrist flex | 360/4,095 degree (degrees) | 1413 | 1418 |
| wrist roll | 360/4,095 degree (degrees) | 1366 | 1357 |
| gripper | 0.06983 of LeRobot's calibrated scale (a range of 1432 counts) | 1405 | 988 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (90,274 frames); every frame then read as it would arrive. The held-out half (108,356 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 53 | 221 | 0 |
| shoulder lift | 78 | 257 | 0 |
| elbow flex | 56 | 268 | 1 |
| wrist flex | 56 | 223 | 0 |
| wrist roll | 37 | 152 | 0 |
| gripper | 35 | 156 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.526 | 0.896 | 2.147 | 1.418 |
| shoulder lift | 1.330 | 1.681 | 3.026 | 2.199 |
| elbow flex | 1.617 | 2.111 | 4.197 | 3.067 |
| wrist flex | 3.456 | 3.483 | 1.641 | 1.190 |
| wrist roll | 0.348 | 0.450 | 1.023 | 0.704 |
| gripper | 9.698 | 10.628 | 16.018 | 14.825 |
| five body joints | 1.829 | 2.020 | 2.649 | 1.905 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
