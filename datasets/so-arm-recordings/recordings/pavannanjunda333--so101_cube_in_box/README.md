# pavannanjunda333/so101_cube_in_box

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [pavannanjunda333/so101_cube_in_box](https://huggingface.co/datasets/pavannanjunda333/so101_cube_in_box), revision `2eba44c1774fb6551adb34b2c9058d24277de764` |
| Author | [pavannanjunda333](https://huggingface.co/pavannanjunda333) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 190 episodes, 92,751 frames |
| Rank | 219 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1688 | 1724 |
| shoulder lift | 360/4,095 degree (degrees) | 2040 | 2058 |
| elbow flex | 360/4,095 degree (degrees) | 2178 | 2190 |
| wrist flex | 360/4,095 degree (degrees) | 1289 | 1331 |
| wrist roll | 360/4,095 degree (degrees) | 1451 | 1468 |
| gripper | 0.06357 of LeRobot's calibrated scale (a range of 1573 counts) | 841 | 695 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (46,858 frames); every frame then read as it would arrive. The held-out half (45,893 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 84 | 141 | 16 |
| shoulder lift | 34 | 158 | 25 |
| elbow flex | 69 | 173 | 27 |
| wrist flex | 45 | 188 | 0 |
| wrist roll | 79 | 157 | 51 |
| gripper | 21 | 125 | 2 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.472 | 0.935 | 2.369 | 1.503 |
| shoulder lift | 1.684 | 2.363 | 4.265 | 2.958 |
| elbow flex | 1.149 | 2.191 | 4.916 | 3.498 |
| wrist flex | 2.382 | 2.752 | 3.407 | 2.303 |
| wrist roll | 0.550 | 0.676 | 2.078 | 1.252 |
| gripper | 8.813 | 8.831 | 9.279 | 8.992 |
| five body joints | 1.439 | 1.964 | 3.574 | 2.453 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
