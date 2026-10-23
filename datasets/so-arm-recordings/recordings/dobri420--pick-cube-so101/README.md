# dobri420/pick-cube-so101

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [dobri420/pick-cube-so101](https://huggingface.co/datasets/dobri420/pick-cube-so101), revision `87952448c358950c406891e0dc0a04433b281497` |
| Author | Dobromir Yordanov ([dobri420](https://huggingface.co/dobri420)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Built from (as its card credits) | [dobri420/pick-cube-so101-sim](https://huggingface.co/datasets/dobri420/pick-cube-so101-sim), by Dobromir Yordanov (dobri420), apache-2.0 |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 540 episodes, 207,265 frames |
| Rank | 73 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2232 | 2278 |
| shoulder lift | 360/4,095 degree (degrees) | 1886 | 1913 |
| elbow flex | 360/4,095 degree (degrees) | 2023 | 2080 |
| wrist flex | 360/4,095 degree (degrees) | 1103 | 1166 |
| wrist roll | 360/4,095 degree (degrees) | 1319 | 1329 |
| gripper | 0.06969 of LeRobot's calibrated scale (a range of 1435 counts) | 644 | 565 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (101,483 frames); every frame then read as it would arrive. The held-out half (105,782 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 34 | 136 | 1 |
| shoulder lift | 40 | 182 | 21 |
| elbow flex | 45 | 196 | 6 |
| wrist flex | 62 | 114 | 1 |
| wrist roll | 40 | 90 | 120 |
| gripper | 33 | 104 | 16 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 83.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.643 | 1.069 | 1.709 | 1.395 |
| shoulder lift | 2.366 | 5.304 | 7.579 | 6.077 |
| elbow flex | 1.510 | 5.111 | 8.298 | 6.626 |
| wrist flex | 0.733 | 1.802 | 3.057 | 2.314 |
| wrist roll | 0.491 | 1.349 | 2.266 | 1.749 |
| gripper | 7.298 | 7.513 | 8.115 | 7.793 |
| five body joints | 1.346 | 3.477 | 5.361 | 4.270 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
