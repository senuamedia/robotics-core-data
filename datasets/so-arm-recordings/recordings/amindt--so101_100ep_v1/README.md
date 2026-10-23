# amindt/so101_100ep_v1

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [amindt/so101_100ep_v1](https://huggingface.co/datasets/amindt/so101_100ep_v1), revision `a8e18b2683cdcd7194acdb7f9ae250abd9f76f11` |
| Author | [amindt](https://huggingface.co/amindt) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 89,827 frames |
| Rank | 232 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 916 | 950 |
| shoulder lift | 360/4,095 degree (degrees) | 1710 | 1754 |
| elbow flex | 360/4,095 degree (degrees) | 1746 | 1805 |
| wrist flex | 360/4,095 degree (degrees) | 794 | 819 |
| wrist roll | 360/4,095 degree (degrees) | 1274 | 1304 |
| gripper | 0.06775 of LeRobot's calibrated scale (a range of 1476 counts) | 314 | 279 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (44,927 frames); every frame then read as it would arrive. The held-out half (44,900 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 23 | 54 | 47 |
| shoulder lift | 22 | 120 | 0 |
| elbow flex | 13 | 108 | 0 |
| wrist flex | 20 | 73 | 2 |
| wrist roll | 29 | 60 | 9 |
| gripper | 18 | 83 | 5 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.253 | 0.356 | 1.088 | 0.674 |
| shoulder lift | 1.943 | 2.157 | 2.919 | 2.196 |
| elbow flex | 1.771 | 1.894 | 2.164 | 1.662 |
| wrist flex | 0.641 | 0.647 | 0.800 | 0.631 |
| wrist roll | 0.181 | 0.143 | 0.597 | 0.429 |
| gripper | 2.062 | 2.061 | 2.209 | 2.074 |
| five body joints | 1.218 | 1.327 | 1.754 | 1.313 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
