# vicshi97/sock-garbage-merged_3.0

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [vicshi97/sock-garbage-merged_3.0](https://huggingface.co/datasets/vicshi97/sock-garbage-merged_3.0), revision `4d6c88687bb90e9e3a7300ba8cc0ffa012e692a6` |
| Author | [vicshi97](https://huggingface.co/vicshi97) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 260 episodes, 111,830 frames |
| Rank | 170 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 836 | 861 |
| shoulder lift | 360/4,095 degree (degrees) | 2299 | 2328 |
| elbow flex | 360/4,095 degree (degrees) | 2210 | 2205 |
| wrist flex | 360/4,095 degree (degrees) | 2180 | 2178 |
| wrist roll | 360/4,095 degree (degrees) | 2594 | 2621 |
| gripper | 0.06689 of LeRobot's calibrated scale (a range of 1495 counts) | 795 | 704 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (53,869 frames); every frame then read as it would arrive. The held-out half (57,961 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 35 | 107 | 2 |
| shoulder lift | 44 | 143 | 33 |
| elbow flex | 44 | 155 | 64 |
| wrist flex | 58 | 177 | 45 |
| wrist roll | 20 | 162 | 53 |
| gripper | 15 | 158 | 18 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 61.1 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.775 | 1.193 | 2.050 | 1.609 |
| shoulder lift | 2.325 | 4.451 | 6.541 | 5.316 |
| elbow flex | 2.473 | 5.053 | 7.924 | 6.474 |
| wrist flex | 2.345 | 4.551 | 6.792 | 5.438 |
| wrist roll | 2.196 | 4.442 | 6.556 | 5.224 |
| gripper | 2.412 | 3.408 | 5.257 | 4.448 |
| five body joints | 2.119 | 4.176 | 6.307 | 5.091 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
