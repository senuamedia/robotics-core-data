# hungho77/so101-ai20k-multitask

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [hungho77/so101-ai20k-multitask](https://huggingface.co/datasets/hungho77/so101-ai20k-multitask), revision `7634de5170a8330eee67a49fdf3d786e1e79b169` |
| Author | Hung Ho ([hungho77](https://huggingface.co/hungho77)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 455 episodes, 221,916 frames |
| Rank | 66 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1169 | 1198 |
| shoulder lift | 360/4,095 degree (degrees) | 1918 | 1923 |
| elbow flex | 360/4,095 degree (degrees) | 2021 | 2043 |
| wrist flex | 360/4,095 degree (degrees) | 1001 | 1025 |
| wrist roll | 360/4,095 degree (degrees) | 89 | 98 |
| gripper | 0.06345 of LeRobot's calibrated scale (a range of 1576 counts) | 1022 | 822 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (78,912 frames); every frame then read as it would arrive. The held-out half (143,004 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 46 | 96 | 50 |
| shoulder lift | 68 | 138 | 1 |
| elbow flex | 52 | 154 | 2 |
| wrist flex | 45 | 113 | 0 |
| wrist roll | 9 | 15 | 0 |
| gripper | 28 | 133 | 8 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.323 | 0.612 | 1.546 | 1.019 |
| shoulder lift | 1.176 | 1.992 | 4.527 | 2.999 |
| elbow flex | 1.201 | 1.815 | 4.252 | 3.276 |
| wrist flex | 0.441 | 0.449 | 1.357 | 0.875 |
| wrist roll | 0.175 | 0.176 | 0.363 | 0.361 |
| gripper | 3.157 | 3.433 | 5.040 | 3.941 |
| five body joints | 0.794 | 1.254 | 2.930 | 2.081 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
