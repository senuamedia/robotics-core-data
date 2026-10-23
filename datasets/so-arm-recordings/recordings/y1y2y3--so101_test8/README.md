# y1y2y3/so101_test8

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [y1y2y3/so101_test8](https://huggingface.co/datasets/y1y2y3/so101_test8), revision `c1818a633ce9fe6b1ef0dd4246f49defc94edd10` |
| Author | [y1y2y3](https://huggingface.co/y1y2y3) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 250 episodes, 111,973 frames |
| Rank | 169 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07334 of LeRobot's calibrated scale (a range of 2727 counts) | 1239 | 1220 |
| shoulder lift | 0.08418 of LeRobot's calibrated scale (a range of 2376 counts) | 1781 | 1885 |
| elbow flex | 0.08850 of LeRobot's calibrated scale (a range of 2260 counts) | 1736 | 1720 |
| wrist flex | 0.11099 of LeRobot's calibrated scale (a range of 1802 counts) | 400 | 469 |
| wrist roll | 0.05214 of LeRobot's calibrated scale (a range of 3836 counts) | 2051 | 1950 |
| gripper | 0.07435 of LeRobot's calibrated scale (a range of 1345 counts) | 735 | 699 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (56,249 frames); every frame then read as it would arrive. The held-out half (55,724 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 34 | 125 | 27 |
| shoulder lift | 52 | 184 | 48 |
| elbow flex | 41 | 181 | 98 |
| wrist flex | 21 | 114 | 44 |
| wrist roll | 64 | 194 | 4 |
| gripper | 17 | 193 | 0 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.320 | 0.796 | 2.048 | 1.310 |
| shoulder lift | 1.839 | 2.300 | 3.091 | 2.310 |
| elbow flex | 1.108 | 1.242 | 3.564 | 2.846 |
| wrist flex | 0.238 | 0.202 | 0.676 | 0.497 |
| wrist roll | 0.348 | 0.464 | 1.258 | 0.827 |
| gripper | 1.016 | 1.152 | 1.843 | 1.483 |
| five body joints | 0.988 | 1.242 | 2.387 | 1.793 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
