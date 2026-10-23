# BoiWanKenobi/lerobot_so101_put_in_pot

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [BoiWanKenobi/lerobot_so101_put_in_pot](https://huggingface.co/datasets/BoiWanKenobi/lerobot_so101_put_in_pot), revision `892ef845c606dbe0c9a2d27be834795fdf20ddc5` |
| Author | [BoiWanKenobi](https://huggingface.co/BoiWanKenobi) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 161 episodes, 81,993 frames |
| Rank | 260 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09398 of LeRobot's calibrated scale (a range of 2128 counts) | 1793 | 1612 |
| shoulder lift | 0.10499 of LeRobot's calibrated scale (a range of 1905 counts) | 1906 | 2357 |
| elbow flex | 0.11669 of LeRobot's calibrated scale (a range of 1714 counts) | 1712 | 1945 |
| wrist flex | 0.10406 of LeRobot's calibrated scale (a range of 1922 counts) | 1450 | 1278 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 2007 | 2070 |
| gripper | 0.16260 of LeRobot's calibrated scale (a range of 615 counts) | 572 | 690 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (40,249 frames); every frame then read as it would arrive. The held-out half (31,309 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 24 | 142 | 2 |
| shoulder lift | 16 | 187 | 43 |
| elbow flex | 36 | 203 | 61 |
| wrist flex | 68 | 178 | 44 |
| wrist roll | 57 | 156 | 14 |
| gripper | 4 | 170 | 47 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.899 | 0.940 | 2.512 | 1.556 |
| shoulder lift | 2.653 | 2.988 | 3.824 | 2.445 |
| elbow flex | 1.588 | 2.511 | 5.772 | 3.995 |
| wrist flex | 0.722 | 1.565 | 4.203 | 2.653 |
| wrist roll | 0.431 | 0.615 | 1.813 | 1.143 |
| gripper | 6.323 | 6.224 | 7.022 | 6.432 |
| five body joints | 1.487 | 1.946 | 3.878 | 2.557 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
