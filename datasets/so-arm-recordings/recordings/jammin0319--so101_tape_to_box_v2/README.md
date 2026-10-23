# jammin0319/so101_tape_to_box_v2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [jammin0319/so101_tape_to_box_v2](https://huggingface.co/datasets/jammin0319/so101_tape_to_box_v2), revision `bb465907ffb5e99e0e0b99e311916575f29b967a` |
| Author | [jammin0319](https://huggingface.co/jammin0319) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 91,889 frames |
| Rank | 221 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1436 | 1534 |
| shoulder lift | 360/4,095 degree (degrees) | 2088 | 2092 |
| elbow flex | 360/4,095 degree (degrees) | 2079 | 2121 |
| wrist flex | 360/4,095 degree (degrees) | 907 | 935 |
| wrist roll | 360/4,095 degree (degrees) | 225 | 282 |
| gripper | 0.06527 of LeRobot's calibrated scale (a range of 1532 counts) | 669 | 614 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (48,696 frames); every frame then read as it would arrive. The held-out half (43,193 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 33 | 60 | 36 |
| shoulder lift | 66 | 105 | 11 |
| elbow flex | 52 | 137 | 37 |
| wrist flex | 37 | 86 | 36 |
| wrist roll | 18 | 41 | 3 |
| gripper | 15 | 115 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.278 | 0.408 | 1.077 | 0.788 |
| shoulder lift | 1.627 | 1.955 | 1.972 | 1.445 |
| elbow flex | 1.408 | 1.674 | 2.923 | 2.185 |
| wrist flex | 2.544 | 2.641 | 2.241 | 1.756 |
| wrist roll | 1.024 | 1.025 | 1.011 | 1.010 |
| gripper | 3.505 | 3.573 | 3.912 | 3.585 |
| five body joints | 1.563 | 1.721 | 1.981 | 1.522 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
