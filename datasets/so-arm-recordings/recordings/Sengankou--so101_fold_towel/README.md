# Sengankou/so101_fold_towel

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Sengankou/so101_fold_towel](https://huggingface.co/datasets/Sengankou/so101_fold_towel), revision `d7dd3d4242e52c38880397ee9da8587f3a1d5801` |
| Author | Koya Sato ([Sengankou](https://huggingface.co/Sengankou)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 180 episodes, 238,127 frames |
| Rank | 58 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07372 of LeRobot's calibrated scale (a range of 2713 counts) | 1331 | 1366 |
| shoulder lift | 0.08500 of LeRobot's calibrated scale (a range of 2353 counts) | 2020 | 2048 |
| elbow flex | 0.09074 of LeRobot's calibrated scale (a range of 2204 counts) | 1897 | 1965 |
| wrist flex | 0.08684 of LeRobot's calibrated scale (a range of 2303 counts) | 1289 | 1349 |
| wrist roll | 0.05238 of LeRobot's calibrated scale (a range of 3818 counts) | 2147 | 2211 |
| gripper | 0.06707 of LeRobot's calibrated scale (a range of 1491 counts) | 619 | 556 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (125,385 frames); every frame then read as it would arrive. The held-out half (112,742 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 49 | 80 | 8 |
| shoulder lift | 58 | 94 | 8 |
| elbow flex | 42 | 105 | 23 |
| wrist flex | 60 | 110 | 7 |
| wrist roll | 42 | 160 | 0 |
| gripper | 26 | 135 | 13 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.410 | 0.741 | 1.444 | 1.124 |
| shoulder lift | 1.513 | 2.275 | 3.738 | 2.830 |
| elbow flex | 1.086 | 1.954 | 3.818 | 2.953 |
| wrist flex | 0.580 | 1.413 | 2.931 | 2.062 |
| wrist roll | 0.441 | 1.105 | 2.319 | 1.611 |
| gripper | 1.661 | 2.076 | 3.229 | 2.490 |
| five body joints | 0.913 | 1.597 | 2.986 | 2.228 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
