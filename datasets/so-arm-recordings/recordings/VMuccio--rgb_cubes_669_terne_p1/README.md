# VMuccio/rgb_cubes_669_terne_p1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [VMuccio/rgb_cubes_669_terne_p1](https://huggingface.co/datasets/VMuccio/rgb_cubes_669_terne_p1), revision `1cdd672fc68872bab048ca12100856c094522efe` |
| Author | Valeria Muccio ([VMuccio](https://huggingface.co/VMuccio)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 750 episodes, 635,370 frames |
| Rank | 17 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 862 | 887 |
| shoulder lift | 360/4,095 degree (degrees) | 1592 | 1603 |
| elbow flex | 360/4,095 degree (degrees) | 1524 | 1575 |
| wrist flex | 360/4,095 degree (degrees) | 1028 | 1044 |
| wrist roll | 360/4,095 degree (degrees) | 633 | 647 |
| gripper | 0.06523 of LeRobot's calibrated scale (a range of 1533 counts) | 582 | 520 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (326,028 frames); every frame then read as it would arrive. The held-out half (309,342 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 45 | 96 | 10 |
| shoulder lift | 49 | 111 | 2 |
| elbow flex | 77 | 91 | 2 |
| wrist flex | 67 | 101 | 7 |
| wrist roll | 53 | 77 | 9 |
| gripper | 28 | 141 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.774 | 0.773 | 1.176 | 0.952 |
| shoulder lift | 0.965 | 1.309 | 2.634 | 2.063 |
| elbow flex | 1.238 | 1.450 | 2.992 | 2.749 |
| wrist flex | 0.416 | 0.297 | 0.899 | 0.641 |
| wrist roll | 0.163 | 0.162 | 0.305 | 0.298 |
| gripper | 2.794 | 2.810 | 3.148 | 2.838 |
| five body joints | 0.807 | 0.951 | 1.906 | 1.625 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
