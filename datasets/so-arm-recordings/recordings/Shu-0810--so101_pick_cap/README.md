# Shu-0810/so101_pick_cap

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Shu-0810/so101_pick_cap](https://huggingface.co/datasets/Shu-0810/so101_pick_cap), revision `8d7195f67cfe1af00278f591a46b78a8e998984e` |
| Author | [Shu-0810](https://huggingface.co/Shu-0810) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 103 episodes, 101,330 frames |
| Rank | 198 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2267 | 2315 |
| shoulder lift | 360/4,095 degree (degrees) | 2081 | 2064 |
| elbow flex | 360/4,095 degree (degrees) | 2193 | 2240 |
| wrist flex | 360/4,095 degree (degrees) | 2300 | 2329 |
| wrist roll | 360/4,095 degree (degrees) | 1046 | 1038 |
| gripper | 0.06583 of LeRobot's calibrated scale (a range of 1519 counts) | 1441 | 1404 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (45,722 frames); every frame then read as it would arrive. The held-out half (55,608 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 34 | 121 | 675 |
| shoulder lift | 34 | 127 | 23 |
| elbow flex | 42 | 134 | 149 |
| wrist flex | 49 | 173 | 31 |
| wrist roll | 40 | 93 | 60 |
| gripper | 39 | 132 | 3 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.589 | 2.111 | 3.805 | 2.736 |
| shoulder lift | 1.230 | 2.438 | 3.929 | 3.170 |
| elbow flex | 1.180 | 2.453 | 4.565 | 3.564 |
| wrist flex | 5.988 | 6.793 | 5.633 | 4.376 |
| wrist roll | 0.825 | 1.271 | 2.170 | 1.714 |
| gripper | 2.773 | 3.381 | 5.702 | 5.181 |
| five body joints | 2.821 | 3.582 | 4.176 | 3.235 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
