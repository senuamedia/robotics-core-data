# eundong123/so101

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [eundong123/so101](https://huggingface.co/datasets/eundong123/so101), revision `6eefe2fe89b8c7c54bdbdf931a27eabc5f563644` |
| Author | [eundong123](https://huggingface.co/eundong123) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 180 episodes, 74,761 frames |
| Rank | 291 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1240 | 1274 |
| shoulder lift | 360/4,095 degree (degrees) | 1927 | 1951 |
| elbow flex | 360/4,095 degree (degrees) | 1883 | 1998 |
| wrist flex | 360/4,095 degree (degrees) | 1182 | 1229 |
| wrist roll | 360/4,095 degree (degrees) | 733 | 756 |
| gripper | 0.06443 of LeRobot's calibrated scale (a range of 1552 counts) | 1197 | 1242 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (37,981 frames); every frame then read as it would arrive. The held-out half (36,780 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 41 | 97 | 11 |
| shoulder lift | 27 | 141 | 27 |
| elbow flex | 14 | 160 | 10 |
| wrist flex | 36 | 121 | 8 |
| wrist roll | 18 | 98 | 30 |
| gripper | 21 | 157 | 9 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.583 | 0.941 | 1.937 | 1.358 |
| shoulder lift | 1.394 | 2.309 | 4.502 | 3.149 |
| elbow flex | 1.400 | 1.966 | 4.173 | 3.356 |
| wrist flex | 0.841 | 1.202 | 2.244 | 1.619 |
| wrist roll | 0.213 | 0.214 | 0.339 | 0.334 |
| gripper | 3.734 | 4.178 | 6.357 | 5.275 |
| five body joints | 0.999 | 1.521 | 3.052 | 2.269 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
