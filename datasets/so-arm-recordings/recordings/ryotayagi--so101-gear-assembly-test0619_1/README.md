# ryotayagi/so101-gear-assembly-test0619_1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [ryotayagi/so101-gear-assembly-test0619_1](https://huggingface.co/datasets/ryotayagi/so101-gear-assembly-test0619_1), revision `3932a3be844b453210f20b15d08f620b1b411097` |
| Author | [ryotayagi](https://huggingface.co/ryotayagi) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 114,169 frames |
| Rank | 165 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07663 of LeRobot's calibrated scale (a range of 2610 counts) | 882 | 889 |
| shoulder lift | 0.08319 of LeRobot's calibrated scale (a range of 2404 counts) | 1815 | 1822 |
| elbow flex | 0.09025 of LeRobot's calibrated scale (a range of 2216 counts) | 1580 | 1645 |
| wrist flex | 0.09814 of LeRobot's calibrated scale (a range of 2038 counts) | 718 | 842 |
| wrist roll | 0.04885 of LeRobot's calibrated scale (a range of 4094 counts) | 772 | 760 |
| gripper | 0.06566 of LeRobot's calibrated scale (a range of 1523 counts) | 608 | 597 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (131,138 frames); every frame then read as it would arrive. The held-out half (97,200 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 65 | 66 | 11 |
| shoulder lift | 67 | 113 | 9 |
| elbow flex | 50 | 81 | 161 |
| wrist flex | 39 | 58 | 24 |
| wrist roll | 73 | 74 | 16 |
| gripper | 17 | 129 | 3 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 161.1 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.029 | 0.039 | 0.195 | 0.192 |
| shoulder lift | 0.435 | 0.575 | 0.985 | 0.836 |
| elbow flex | 0.613 | 0.613 | 1.240 | 1.232 |
| wrist flex | 0.304 | 0.479 | 0.758 | 0.605 |
| wrist roll | 0.000 | 0.000 | 0.346 | 0.346 |
| gripper | 0.013 | 0.021 | 0.270 | 0.269 |
| five body joints | 0.362 | 0.433 | 0.805 | 0.740 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
