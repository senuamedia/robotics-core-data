# shanuka15/so101_RGB_finalmerge

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [shanuka15/so101_RGB_finalmerge](https://huggingface.co/datasets/shanuka15/so101_RGB_finalmerge), revision `838272f37b0811ccf8bd09537f310b8e0f0bd047` |
| Author | S ([shanuka15](https://huggingface.co/shanuka15)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 303 episodes, 234,405 frames |
| Rank | 61 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1401 | 1427 |
| shoulder lift | 360/4,095 degree (degrees) | 1939 | 2037 |
| elbow flex | 360/4,095 degree (degrees) | 1971 | 2031 |
| wrist flex | 360/4,095 degree (degrees) | 1111 | 1134 |
| wrist roll | 360/4,095 degree (degrees) | 1348 | 1377 |
| gripper | 0.06680 of LeRobot's calibrated scale (a range of 1497 counts) | 891 | 776 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (114,870 frames); every frame then read as it would arrive. The held-out half (119,535 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 47 | 75 | 1 |
| shoulder lift | 57 | 107 | 0 |
| elbow flex | 60 | 104 | 13 |
| wrist flex | 46 | 110 | 3 |
| wrist roll | 42 | 111 | 13 |
| gripper | 15 | 123 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.000 | 0.000 | 0.175 | 0.174 |
| shoulder lift | 2.890 | 2.893 | 2.724 | 2.721 |
| elbow flex | 0.526 | 0.526 | 0.175 | 0.174 |
| wrist flex | 0.087 | 0.087 | 0.175 | 0.174 |
| wrist roll | 0.087 | 0.087 | 0.439 | 0.438 |
| gripper | 0.000 | 0.000 | 0.087 | 0.087 |
| five body joints | 1.314 | 1.315 | 1.241 | 1.240 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
