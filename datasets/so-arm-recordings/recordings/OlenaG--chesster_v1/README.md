# OlenaG/chesster_v1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [OlenaG/chesster_v1](https://huggingface.co/datasets/OlenaG/chesster_v1), revision `b1896a8afd68512117d57566036386ff5ee49ae2` |
| Author | Olena G ([OlenaG](https://huggingface.co/OlenaG)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 201 episodes, 210,526 frames |
| Rank | 71 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1804 | 1842 |
| shoulder lift | 360/4,095 degree (degrees) | 1895 | 1911 |
| elbow flex | 360/4,095 degree (degrees) | 1890 | 1939 |
| wrist flex | 360/4,095 degree (degrees) | 856 | 867 |
| wrist roll | 360/4,095 degree (degrees) | 1716 | 1737 |
| gripper | 0.06636 of LeRobot's calibrated scale (a range of 1507 counts) | 312 | 289 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (110,995 frames); every frame then read as it would arrive. The held-out half (99,531 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 57 | 99 | 13 |
| shoulder lift | 60 | 90 | 2 |
| elbow flex | 60 | 109 | 0 |
| wrist flex | 48 | 83 | 12 |
| wrist roll | 18 | 113 | 50 |
| gripper | 30 | 98 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.316 | 0.476 | 1.183 | 0.830 |
| shoulder lift | 0.543 | 0.796 | 1.331 | 0.910 |
| elbow flex | 1.158 | 1.444 | 2.716 | 2.197 |
| wrist flex | 0.551 | 0.653 | 1.004 | 0.706 |
| wrist roll | 0.319 | 0.852 | 1.848 | 1.233 |
| gripper | 0.220 | 0.217 | 0.479 | 0.351 |
| five body joints | 0.654 | 0.905 | 1.730 | 1.293 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
