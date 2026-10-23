# armandomm09/so101_terminal_sort

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [armandomm09/so101_terminal_sort](https://huggingface.co/datasets/armandomm09/so101_terminal_sort), revision `d0279be86407844cc47d8e72b5cec3ffc3ad8e7a` |
| Author | [armandomm09](https://huggingface.co/armandomm09) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 232 episodes, 103,422 frames |
| Rank | 195 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 796 | 805 |
| shoulder lift | 360/4,095 degree (degrees) | 1750 | 1770 |
| elbow flex | 360/4,095 degree (degrees) | 1866 | 1918 |
| wrist flex | 360/4,095 degree (degrees) | 1233 | 1262 |
| wrist roll | 360/4,095 degree (degrees) | 835 | 842 |
| gripper | 0.06515 of LeRobot's calibrated scale (a range of 1535 counts) | 588 | 450 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (54,189 frames); every frame then read as it would arrive. The held-out half (49,233 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 39 | 99 | 6 |
| shoulder lift | 29 | 151 | 0 |
| elbow flex | 32 | 161 | 3 |
| wrist flex | 20 | 155 | 0 |
| wrist roll | 15 | 119 | 1 |
| gripper | 15 | 133 | 1 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.377 | 0.244 | 0.850 | 0.451 |
| shoulder lift | 1.043 | 1.197 | 2.414 | 1.688 |
| elbow flex | 0.896 | 1.271 | 3.145 | 2.381 |
| wrist flex | 0.881 | 0.877 | 2.157 | 1.278 |
| wrist roll | 0.487 | 0.284 | 0.949 | 0.519 |
| gripper | 1.840 | 1.823 | 3.606 | 2.759 |
| five body joints | 0.780 | 0.889 | 2.097 | 1.458 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
