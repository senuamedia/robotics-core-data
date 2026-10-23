# guloyy/SO101-eval2-rgb-expanded-v3-256

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [guloyy/SO101-eval2-rgb-expanded-v3-256](https://huggingface.co/datasets/guloyy/SO101-eval2-rgb-expanded-v3-256), revision `73fa571acbed69d0621fd431640d9b6743237ef9` |
| Author | Gül Oymak ([guloyy](https://huggingface.co/guloyy)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 300 episodes, 145,110 frames |
| Rank | 116 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 720 | 736 |
| shoulder lift | 360/4,095 degree (degrees) | 1144 | 1147 |
| elbow flex | 360/4,095 degree (degrees) | 812 | 887 |
| wrist flex | 360/4,095 degree (degrees) | 734 | 762 |
| wrist roll | 360/4,095 degree (degrees) | 988 | 1011 |
| gripper | 0.06978 of LeRobot's calibrated scale (a range of 1433 counts) | 430 | 421 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (71,770 frames); every frame then read as it would arrive. The held-out half (73,340 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 30 | 51 | 200 |
| shoulder lift | 48 | 76 | 40 |
| elbow flex | 40 | 66 | 60 |
| wrist flex | 45 | 69 | 0 |
| wrist roll | 43 | 66 | 0 |
| gripper | 45 | 119 | 60 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.208 | 0.217 | 0.556 | 0.452 |
| shoulder lift | 1.846 | 2.024 | 1.599 | 1.306 |
| elbow flex | 1.394 | 1.454 | 2.466 | 2.307 |
| wrist flex | 0.798 | 0.820 | 1.183 | 0.936 |
| wrist roll | 0.304 | 0.300 | 1.173 | 0.714 |
| gripper | 3.199 | 3.228 | 3.448 | 3.289 |
| five body joints | 1.106 | 1.184 | 1.531 | 1.312 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
