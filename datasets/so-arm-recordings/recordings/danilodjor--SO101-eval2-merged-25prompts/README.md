# danilodjor/SO101-eval2-merged-25prompts

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [danilodjor/SO101-eval2-merged-25prompts](https://huggingface.co/datasets/danilodjor/SO101-eval2-merged-25prompts), revision `e31ac20977578cdeb1cdd225bdc46a163c6f27b9` |
| Author | Danilo Dordevic ([danilodjor](https://huggingface.co/danilodjor)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 1,518 episodes, 671,081 frames |
| Rank | 16 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 907 | 922 |
| shoulder lift | 360/4,095 degree (degrees) | 1587 | 1604 |
| elbow flex | 360/4,095 degree (degrees) | 1002 | 1076 |
| wrist flex | 360/4,095 degree (degrees) | 978 | 1010 |
| wrist roll | 360/4,095 degree (degrees) | 1239 | 1255 |
| gripper | 0.06978 of LeRobot's calibrated scale (a range of 1433 counts) | 620 | 540 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (338,291 frames); every frame then read as it would arrive. The held-out half (332,790 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 50 | 122 | 8 |
| shoulder lift | 74 | 180 | 120 |
| elbow flex | 69 | 147 | 272 |
| wrist flex | 56 | 153 | 179 |
| wrist roll | 72 | 139 | 64 |
| gripper | 54 | 170 | 48 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.360 | 0.296 | 0.924 | 0.638 |
| shoulder lift | 1.623 | 1.937 | 2.028 | 1.454 |
| elbow flex | 1.144 | 1.324 | 3.215 | 2.869 |
| wrist flex | 0.914 | 0.893 | 1.266 | 0.915 |
| wrist roll | 0.791 | 0.682 | 1.829 | 1.134 |
| gripper | 2.830 | 2.847 | 3.076 | 2.807 |
| five body joints | 1.052 | 1.171 | 2.012 | 1.604 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
