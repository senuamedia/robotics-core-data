# polrolnik2/so101_candy

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [polrolnik2/so101_candy](https://huggingface.co/datasets/polrolnik2/so101_candy), revision `6fc8a48159bb7416c962cd3c3ea9ce8f6e23c8fb` |
| Author | [polrolnik2](https://huggingface.co/polrolnik2) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 84,865 frames |
| Rank | 252 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2127 | 2155 |
| shoulder lift | 360/4,095 degree (degrees) | 1965 | 2001 |
| elbow flex | 360/4,095 degree (degrees) | 2150 | 2197 |
| wrist flex | 360/4,095 degree (degrees) | 1832 | 1854 |
| wrist roll | 360/4,095 degree (degrees) | 2298 | 2316 |
| gripper | 0.06614 of LeRobot's calibrated scale (a range of 1512 counts) | 613 | 562 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (45,342 frames); every frame then read as it would arrive. The held-out half (39,523 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 43 | 143 | 33 |
| shoulder lift | 32 | 181 | 5 |
| elbow flex | 21 | 197 | 2 |
| wrist flex | 52 | 186 | 104 |
| wrist roll | 37 | 228 | 5 |
| gripper | 10 | 116 | 3 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.406 | 0.487 | 1.217 | 0.824 |
| shoulder lift | 1.297 | 2.123 | 4.020 | 2.730 |
| elbow flex | 1.073 | 1.577 | 3.502 | 2.653 |
| wrist flex | 1.233 | 2.377 | 4.731 | 3.228 |
| wrist roll | 0.538 | 0.984 | 2.351 | 1.512 |
| gripper | 7.073 | 7.079 | 7.177 | 7.113 |
| five body joints | 0.980 | 1.664 | 3.400 | 2.361 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
