# phonglnd-prdcv/pnp_single_block_2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [phonglnd-prdcv/pnp_single_block_2](https://huggingface.co/datasets/phonglnd-prdcv/pnp_single_block_2), revision `228437b4f4dad82b6fac9321ce928fc4912fa97e` |
| Author | Phong Lam ([phonglnd-prdcv](https://huggingface.co/phonglnd-prdcv)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 150 episodes, 123,272 frames |
| Rank | 143 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 811 | 833 |
| shoulder lift | 360/4,095 degree (degrees) | 1840 | 1840 |
| elbow flex | 360/4,095 degree (degrees) | 1704 | 1761 |
| wrist flex | 360/4,095 degree (degrees) | 766 | 804 |
| wrist roll | 360/4,095 degree (degrees) | 866 | 876 |
| gripper | 0.05774 of LeRobot's calibrated scale (a range of 1732 counts) | 371 | 356 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (65,870 frames); every frame then read as it would arrive. The held-out half (57,402 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 38 | 42 | 49 |
| shoulder lift | 58 | 93 | 183 |
| elbow flex | 46 | 108 | 18 |
| wrist flex | 40 | 82 | 4 |
| wrist roll | 34 | 54 | 16 |
| gripper | 16 | 78 | 13 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.319 | 0.397 | 0.783 | 0.666 |
| shoulder lift | 1.686 | 1.916 | 1.756 | 1.461 |
| elbow flex | 1.389 | 1.866 | 3.979 | 3.543 |
| wrist flex | 0.429 | 0.587 | 1.342 | 0.996 |
| wrist roll | 0.288 | 0.279 | 0.673 | 0.560 |
| gripper | 3.810 | 3.853 | 4.041 | 3.946 |
| five body joints | 1.014 | 1.243 | 2.087 | 1.813 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
