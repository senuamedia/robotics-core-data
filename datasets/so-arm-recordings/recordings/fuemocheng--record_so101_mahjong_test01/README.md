# fuemocheng/record_so101_mahjong_test01

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [fuemocheng/record_so101_mahjong_test01](https://huggingface.co/datasets/fuemocheng/record_so101_mahjong_test01), revision `cdf192276dfbbdd0851b83992aeff5158b7ebe09` |
| Author | [fuemocheng](https://huggingface.co/fuemocheng) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v2.1, 30 frames a second |
| Recording | 60 episodes, 87,855 frames |
| Rank | 241 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09025 of LeRobot's calibrated scale (a range of 2216 counts) | 889 | 918 |
| shoulder lift | 0.08435 of LeRobot's calibrated scale (a range of 2371 counts) | 1938 | 1942 |
| elbow flex | 0.09054 of LeRobot's calibrated scale (a range of 2209 counts) | 1878 | 1898 |
| wrist flex | 0.08673 of LeRobot's calibrated scale (a range of 2306 counts) | 2149 | 2089 |
| wrist roll | 0.08478 of LeRobot's calibrated scale (a range of 2359 counts) | 1676 | 1664 |
| gripper | 0.08651 of LeRobot's calibrated scale (a range of 1156 counts) | 507 | 573 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,660 frames); every frame then read as it would arrive. The held-out half (43,195 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 30 | 56 | 18 |
| shoulder lift | 27 | 78 | 21 |
| elbow flex | 38 | 86 | 21 |
| wrist flex | 30 | 114 | 19 |
| wrist roll | 31 | 105 | 27 |
| gripper | 22 | 96 | 7 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 50.0 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.256 | 0.248 | 0.525 | 0.460 |
| shoulder lift | 1.761 | 2.116 | 2.788 | 2.247 |
| elbow flex | 0.712 | 1.419 | 3.035 | 2.365 |
| wrist flex | 1.117 | 1.405 | 2.315 | 1.609 |
| wrist roll | 0.314 | 1.043 | 2.168 | 1.524 |
| gripper | 1.644 | 1.682 | 1.936 | 1.756 |
| five body joints | 1.001 | 1.386 | 2.337 | 1.775 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
