# lipsop/so101-block-in-bin-100ep

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [lipsop/so101-block-in-bin-100ep](https://huggingface.co/datasets/lipsop/so101-block-in-bin-100ep), revision `2fd6bf360927cadef62a15b4f1a77aa0ba468a8c` |
| Author | [lipsop](https://huggingface.co/lipsop) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 100 episodes, 85,514 frames |
| Rank | 251 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07642 of LeRobot's calibrated scale (a range of 2617 counts) | 1590 | 1535 |
| shoulder lift | 0.08302 of LeRobot's calibrated scale (a range of 2409 counts) | 2081 | 2206 |
| elbow flex | 0.08953 of LeRobot's calibrated scale (a range of 2234 counts) | 2144 | 2231 |
| wrist flex | 0.08319 of LeRobot's calibrated scale (a range of 2404 counts) | 991 | 1021 |
| wrist roll | 0.05137 of LeRobot's calibrated scale (a range of 3893 counts) | 1618 | 1630 |
| gripper | 0.06562 of LeRobot's calibrated scale (a range of 1524 counts) | 736 | 687 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (41,875 frames); every frame then read as it would arrive. The held-out half (43,639 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 37 | 130 | 20 |
| shoulder lift | 19 | 184 | 55 |
| elbow flex | 25 | 208 | 19 |
| wrist flex | 53 | 133 | 107 |
| wrist roll | 26 | 140 | 9 |
| gripper | 17 | 157 | 109 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 83.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.742 | 2.348 | 3.542 | 2.874 |
| shoulder lift | 2.062 | 4.209 | 6.191 | 5.272 |
| elbow flex | 2.280 | 5.383 | 7.908 | 6.629 |
| wrist flex | 3.318 | 4.206 | 4.633 | 3.837 |
| wrist roll | 1.424 | 4.256 | 6.112 | 4.963 |
| gripper | 3.433 | 4.209 | 5.240 | 4.725 |
| five body joints | 2.147 | 4.195 | 5.869 | 4.885 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
