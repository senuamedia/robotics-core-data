# rahul0512/pill_sort_18_holes_1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [rahul0512/pill_sort_18_holes_1](https://huggingface.co/datasets/rahul0512/pill_sort_18_holes_1), revision `80d88c97014b97788ae462320fb487dfbb2249ac` |
| Author | Rahul Patil ([rahul0512](https://huggingface.co/rahul0512)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 108 episodes, 119,657 frames |
| Rank | 148 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1156 | 1178 |
| shoulder lift | 360/4,095 degree (degrees) | 1795 | 1796 |
| elbow flex | 360/4,095 degree (degrees) | 1807 | 1909 |
| wrist flex | 360/4,095 degree (degrees) | 926 | 938 |
| wrist roll | 360/4,095 degree (degrees) | 1394 | 1418 |
| gripper | 0.06439 of LeRobot's calibrated scale (a range of 1553 counts) | 402 | 355 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (64,094 frames); every frame then read as it would arrive. The held-out half (55,563 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 41 | 43 | 6 |
| shoulder lift | 47 | 55 | 127 |
| elbow flex | 49 | 65 | 84 |
| wrist flex | 52 | 52 | 84 |
| wrist roll | 26 | 57 | 44 |
| gripper | 17 | 81 | 6 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 100.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.699 | 0.737 | 1.042 | 0.960 |
| shoulder lift | 14.601 | 14.712 | 1.422 | 1.255 |
| elbow flex | 2.336 | 2.749 | 4.129 | 3.832 |
| wrist flex | 0.279 | 0.254 | 0.556 | 0.489 |
| wrist roll | 0.577 | 0.886 | 1.610 | 1.260 |
| gripper | 1.136 | 1.139 | 1.406 | 1.404 |
| five body joints | 6.626 | 6.713 | 2.147 | 1.950 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
