# louis126584/so101-test5

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [louis126584/so101-test5](https://huggingface.co/datasets/louis126584/so101-test5), revision `aef5c5b01cef1e0f1a75147f5202004e09e6b8a6` |
| Author | yiming liu ([louis126584](https://huggingface.co/louis126584)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 136 episodes, 117,829 frames |
| Rank | 153 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07350 of LeRobot's calibrated scale (a range of 2721 counts) | 1653 | 1639 |
| shoulder lift | 0.08673 of LeRobot's calibrated scale (a range of 2306 counts) | 1774 | 1827 |
| elbow flex | 0.09062 of LeRobot's calibrated scale (a range of 2207 counts) | 1644 | 1687 |
| wrist flex | 0.08703 of LeRobot's calibrated scale (a range of 2298 counts) | 469 | 490 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 981 | 1018 |
| gripper | 0.06831 of LeRobot's calibrated scale (a range of 1464 counts) | 724 | 607 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (60,996 frames); every frame then read as it would arrive. The held-out half (56,833 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 44 | 107 | 84 |
| shoulder lift | 42 | 93 | 244 |
| elbow flex | 22 | 94 | 217 |
| wrist flex | 11 | 55 | 31 |
| wrist roll | 9 | 82 | 228 |
| gripper | 26 | 119 | 104 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.254 | 0.597 | 1.704 | 1.033 |
| shoulder lift | 1.778 | 2.083 | 3.135 | 2.108 |
| elbow flex | 0.699 | 1.243 | 2.958 | 2.041 |
| wrist flex | 0.231 | 0.230 | 0.288 | 0.287 |
| wrist roll | 0.302 | 0.452 | 1.269 | 0.818 |
| gripper | 4.191 | 4.247 | 4.761 | 4.372 |
| five body joints | 0.878 | 1.139 | 2.153 | 1.444 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
