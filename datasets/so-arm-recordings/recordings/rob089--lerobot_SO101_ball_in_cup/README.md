# rob089/lerobot_SO101_ball_in_cup

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [rob089/lerobot_SO101_ball_in_cup](https://huggingface.co/datasets/rob089/lerobot_SO101_ball_in_cup), revision `ab9074c0244a3fd46cc3079d345376ab55f152c1` |
| Author | [rob089](https://huggingface.co/rob089) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 129 episodes, 79,848 frames |
| Rank | 268 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1354 | 1399 |
| shoulder lift | 360/4,095 degree (degrees) | 1818 | 1850 |
| elbow flex | 360/4,095 degree (degrees) | 1718 | 1817 |
| wrist flex | 360/4,095 degree (degrees) | 877 | 915 |
| wrist roll | 360/4,095 degree (degrees) | 506 | 564 |
| gripper | 0.06743 of LeRobot's calibrated scale (a range of 1483 counts) | 1005 | 929 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (58,335 frames); every frame then read as it would arrive. The held-out half (21,513 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 28 | 110 | 20 |
| shoulder lift | 18 | 152 | 32 |
| elbow flex | 12 | 158 | 65 |
| wrist flex | 28 | 111 | 34 |
| wrist roll | 73 | 79 | 3 |
| gripper | 14 | 229 | 15 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.899 | 0.829 | 1.819 | 1.209 |
| shoulder lift | 1.494 | 1.878 | 3.338 | 2.368 |
| elbow flex | 1.444 | 1.779 | 4.105 | 3.187 |
| wrist flex | 1.049 | 1.074 | 1.614 | 1.335 |
| wrist roll | 1.033 | 1.053 | 1.225 | 1.185 |
| gripper | 17.270 | 17.152 | 17.871 | 17.593 |
| five body joints | 1.207 | 1.388 | 2.661 | 2.020 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
