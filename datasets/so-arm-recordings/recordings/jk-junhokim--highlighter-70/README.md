# jk-junhokim/highlighter-70

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [jk-junhokim/highlighter-70](https://huggingface.co/datasets/jk-junhokim/highlighter-70), revision `28c8b0d0075e2dd471527ffc9f4f849c5a438526` |
| Author | [jk-junhokim](https://huggingface.co/jk-junhokim) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 70 episodes, 79,244 frames |
| Rank | 271 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1606 | 1639 |
| shoulder lift | 360/4,095 degree (degrees) | 2058 | 2091 |
| elbow flex | 360/4,095 degree (degrees) | 2046 | 2133 |
| wrist flex | 360/4,095 degree (degrees) | 1388 | 1575 |
| wrist roll | 360/4,095 degree (degrees) | 2424 | 2419 |
| gripper | 0.06775 of LeRobot's calibrated scale (a range of 1476 counts) | 396 | 335 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (40,692 frames); every frame then read as it would arrive. The held-out half (38,552 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 36 | 84 | 5 |
| shoulder lift | 40 | 105 | 5 |
| elbow flex | 49 | 113 | 0 |
| wrist flex | 55 | 109 | 9 |
| wrist roll | 39 | 106 | 47 |
| gripper | 26 | 109 | 2 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.389 | 0.692 | 1.693 | 1.111 |
| shoulder lift | 1.680 | 2.032 | 2.779 | 2.067 |
| elbow flex | 2.114 | 2.298 | 3.237 | 2.554 |
| wrist flex | 2.175 | 2.337 | 3.080 | 2.562 |
| wrist roll | 0.383 | 0.609 | 1.588 | 1.004 |
| gripper | 2.705 | 2.716 | 2.877 | 2.730 |
| five body joints | 1.569 | 1.773 | 2.571 | 1.980 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
