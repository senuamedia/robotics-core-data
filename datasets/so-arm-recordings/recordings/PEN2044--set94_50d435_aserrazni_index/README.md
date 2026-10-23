# PEN2044/set94_50d435_aserrazni_index

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [PEN2044/set94_50d435_aserrazni_index](https://huggingface.co/datasets/PEN2044/set94_50d435_aserrazni_index), revision `cdec6a918917028d89f6bbb588000fc1cabcc533` |
| Author | [PEN2044](https://huggingface.co/PEN2044) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 144 episodes, 102,162 frames |
| Rank | 196 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 757 | 766 |
| shoulder lift | 360/4,095 degree (degrees) | 2045 | 2096 |
| elbow flex | 360/4,095 degree (degrees) | 1719 | 1754 |
| wrist flex | 360/4,095 degree (degrees) | 894 | 971 |
| wrist roll | 360/4,095 degree (degrees) | 1079 | 1112 |
| gripper | 0.06378 of LeRobot's calibrated scale (a range of 1568 counts) | 472 | 383 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,745 frames); every frame then read as it would arrive. The held-out half (57,417 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 37 | 54 | 0 |
| shoulder lift | 57 | 120 | 0 |
| elbow flex | 60 | 120 | 0 |
| wrist flex | 56 | 89 | 0 |
| wrist roll | 33 | 64 | 3 |
| gripper | 42 | 126 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.342 | 0.395 | 0.875 | 0.740 |
| shoulder lift | 1.567 | 1.837 | 2.558 | 2.268 |
| elbow flex | 0.736 | 0.937 | 2.522 | 2.271 |
| wrist flex | 1.613 | 1.625 | 1.665 | 1.557 |
| wrist roll | 0.496 | 0.559 | 0.985 | 0.886 |
| gripper | 3.427 | 3.426 | 3.631 | 3.592 |
| five body joints | 1.091 | 1.213 | 1.865 | 1.676 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
