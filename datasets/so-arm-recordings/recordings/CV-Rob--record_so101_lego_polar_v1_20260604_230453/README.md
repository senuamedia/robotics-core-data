# CV-Rob/record_so101_lego_polar_v1_20260604_230453

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [CV-Rob/record_so101_lego_polar_v1_20260604_230453](https://huggingface.co/datasets/CV-Rob/record_so101_lego_polar_v1_20260604_230453), revision `b897bb56f8ec31822ae4abe8bd64aa3a0a292e16` |
| Author | Jonas Schramm ([CV-Rob](https://huggingface.co/CV-Rob)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 135 episodes, 139,359 frames |
| Rank | 126 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1007 | 1017 |
| shoulder lift | 360/4,095 degree (degrees) | 1853 | 1856 |
| elbow flex | 360/4,095 degree (degrees) | 1768 | 1813 |
| wrist flex | 360/4,095 degree (degrees) | 1071 | 1097 |
| wrist roll | 360/4,095 degree (degrees) | 2297 | 2317 |
| gripper | 0.07082 of LeRobot's calibrated scale (a range of 1412 counts) | 547 | 491 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (76,842 frames); every frame then read as it would arrive. The held-out half (62,517 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 37 | 65 | 116 |
| shoulder lift | 51 | 69 | 387 |
| elbow flex | 59 | 77 | 164 |
| wrist flex | 38 | 80 | 28 |
| wrist roll | 50 | 80 | 62 |
| gripper | 15 | 103 | 42 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 22.2 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.154 | 0.171 | 0.587 | 0.423 |
| shoulder lift | 2.212 | 2.295 | 1.547 | 1.190 |
| elbow flex | 0.675 | 0.783 | 2.649 | 2.286 |
| wrist flex | 0.319 | 0.357 | 0.749 | 0.470 |
| wrist roll | 0.219 | 0.482 | 1.357 | 0.831 |
| gripper | 0.286 | 0.370 | 0.990 | 0.682 |
| five body joints | 1.050 | 1.119 | 1.560 | 1.243 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
