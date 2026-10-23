# DmitryStrog/so101_pnp_merged_500

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [DmitryStrog/so101_pnp_merged_500](https://huggingface.co/datasets/DmitryStrog/so101_pnp_merged_500), revision `f3289247500a6142b635573469feaa63892dc205` |
| Author | Dmitry Stroganov ([DmitryStrog](https://huggingface.co/DmitryStrog)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 521 episodes, 277,740 frames |
| Rank | 44 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07291 of LeRobot's calibrated scale (a range of 2743 counts) | 1694 | 1708 |
| shoulder lift | 0.08340 of LeRobot's calibrated scale (a range of 2398 counts) | 2154 | 2166 |
| elbow flex | 0.08945 of LeRobot's calibrated scale (a range of 2236 counts) | 2204 | 2221 |
| wrist flex | 0.08432 of LeRobot's calibrated scale (a range of 2372 counts) | 2121 | 2127 |
| wrist roll | 0.10482 of LeRobot's calibrated scale (a range of 1908 counts) | 1859 | 1833 |
| gripper | 0.06575 of LeRobot's calibrated scale (a range of 1521 counts) | 1049 | 932 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (142,115 frames); every frame then read as it would arrive. The held-out half (135,625 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 53 | 154 | 0 |
| shoulder lift | 74 | 146 | 0 |
| elbow flex | 43 | 181 | 2 |
| wrist flex | 40 | 139 | 0 |
| wrist roll | 84 | 123 | 23 |
| gripper | 27 | 149 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.296 | 0.344 | 0.807 | 0.633 |
| shoulder lift | 0.878 | 1.017 | 2.124 | 1.641 |
| elbow flex | 0.806 | 1.015 | 2.910 | 2.712 |
| wrist flex | 0.673 | 0.856 | 2.014 | 1.502 |
| wrist roll | 0.540 | 0.874 | 1.720 | 1.245 |
| gripper | 5.996 | 6.006 | 6.164 | 6.010 |
| five body joints | 0.671 | 0.857 | 2.032 | 1.688 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
