# mertkirgil/so101_renkli_kalem

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [mertkirgil/so101_renkli_kalem](https://huggingface.co/datasets/mertkirgil/so101_renkli_kalem), revision `40f709963ed687066f65eba6fef8d813fef5bf4b` |
| Author | [mertkirgil](https://huggingface.co/mertkirgil) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 150 episodes, 74,233 frames |
| Rank | 298 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.10455 of LeRobot's calibrated scale (a range of 1913 counts) | 686 | 709 |
| shoulder lift | 0.08446 of LeRobot's calibrated scale (a range of 2368 counts) | 1960 | 1976 |
| elbow flex | 0.09038 of LeRobot's calibrated scale (a range of 2213 counts) | 1990 | 2027 |
| wrist flex | 0.08688 of LeRobot's calibrated scale (a range of 2302 counts) | 777 | 784 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 679 | 695 |
| gripper | 0.06752 of LeRobot's calibrated scale (a range of 1481 counts) | 362 | 342 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (39,434 frames); every frame then read as it would arrive. The held-out half (35,257 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 104 | 138 | 1 |
| shoulder lift | 334 | 562 | 12 |
| elbow flex | 282 | 458 | 11 |
| wrist flex | 144 | 215 | 0 |
| wrist roll | 79 | 113 | 3 |
| gripper | 83 | 191 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.265 | 0.292 | 0.617 | 0.482 |
| shoulder lift | 1.854 | 2.206 | 3.023 | 2.512 |
| elbow flex | 1.283 | 1.571 | 2.866 | 2.378 |
| wrist flex | 0.529 | 0.620 | 1.161 | 0.843 |
| wrist roll | 0.213 | 0.196 | 0.595 | 0.444 |
| gripper | 3.723 | 3.756 | 3.858 | 3.763 |
| five body joints | 1.046 | 1.252 | 1.971 | 1.618 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
