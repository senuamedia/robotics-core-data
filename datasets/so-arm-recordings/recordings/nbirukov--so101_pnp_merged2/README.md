# nbirukov/so101_pnp_merged2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [nbirukov/so101_pnp_merged2](https://huggingface.co/datasets/nbirukov/so101_pnp_merged2), revision `390929d146aeec72e190831f4bd4d3a802b13be6` |
| Author | Nikita Birukov ([nbirukov](https://huggingface.co/nbirukov)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 467 episodes, 243,673 frames |
| Rank | 55 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07291 of LeRobot's calibrated scale (a range of 2743 counts) | 1694 | 1708 |
| shoulder lift | 0.08340 of LeRobot's calibrated scale (a range of 2398 counts) | 2154 | 2166 |
| elbow flex | 0.08945 of LeRobot's calibrated scale (a range of 2236 counts) | 2204 | 2221 |
| wrist flex | 0.08432 of LeRobot's calibrated scale (a range of 2372 counts) | 2094 | 2091 |
| wrist roll | 0.10482 of LeRobot's calibrated scale (a range of 1908 counts) | 1859 | 1833 |
| gripper | 0.06575 of LeRobot's calibrated scale (a range of 1521 counts) | 1043 | 929 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (123,743 frames); every frame then read as it would arrive. The held-out half (119,930 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 51 | 148 | 0 |
| shoulder lift | 56 | 145 | 0 |
| elbow flex | 36 | 181 | 2 |
| wrist flex | 64 | 135 | 0 |
| wrist roll | 94 | 121 | 23 |
| gripper | 30 | 149 | 1 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.349 | 0.794 | 1.814 | 1.270 |
| shoulder lift | 1.120 | 1.831 | 3.570 | 2.764 |
| elbow flex | 0.623 | 1.521 | 4.048 | 3.503 |
| wrist flex | 0.493 | 0.680 | 1.437 | 1.085 |
| wrist roll | 0.561 | 1.170 | 2.271 | 1.640 |
| gripper | 5.624 | 5.702 | 6.025 | 5.727 |
| five body joints | 0.681 | 1.275 | 2.815 | 2.253 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
