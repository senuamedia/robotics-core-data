# TzuShian/so101_white_chess_20251021

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [TzuShian/so101_white_chess_20251021](https://huggingface.co/datasets/TzuShian/so101_white_chess_20251021), revision `af16ff85d18e16ec99b3f5b77c833008d22be354` |
| Author | Tzu-Shian Yang ([TzuShian](https://huggingface.co/TzuShian)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 274 episodes, 238,050 frames |
| Rank | 59 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07380 of LeRobot's calibrated scale (a range of 2710 counts) | 1689 | 1685 |
| shoulder lift | 0.08425 of LeRobot's calibrated scale (a range of 2374 counts) | 1910 | 1896 |
| elbow flex | 0.09091 of LeRobot's calibrated scale (a range of 2200 counts) | 1115 | 1150 |
| wrist flex | 0.08639 of LeRobot's calibrated scale (a range of 2315 counts) | 1781 | 1808 |
| wrist roll | 0.05278 of LeRobot's calibrated scale (a range of 3789 counts) | 161 | 168 |
| gripper | 0.06897 of LeRobot's calibrated scale (a range of 1450 counts) | 1425 | 1255 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (119,955 frames); every frame then read as it would arrive. The held-out half (118,095 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 60 | 85 | 6 |
| shoulder lift | 62 | 87 | 5 |
| elbow flex | 42 | 69 | 92 |
| wrist flex | 47 | 102 | 3 |
| wrist roll | 34 | 41 | 3 |
| gripper | 36 | 163 | 10 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.205 | 0.741 | 1.824 | 1.174 |
| shoulder lift | 1.131 | 1.445 | 2.851 | 2.337 |
| elbow flex | 0.696 | 0.811 | 1.544 | 1.226 |
| wrist flex | 0.326 | 1.117 | 2.708 | 1.865 |
| wrist roll | 0.115 | 0.112 | 0.292 | 0.289 |
| gripper | 9.280 | 9.348 | 4.459 | 3.103 |
| five body joints | 0.620 | 0.954 | 2.061 | 1.542 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
