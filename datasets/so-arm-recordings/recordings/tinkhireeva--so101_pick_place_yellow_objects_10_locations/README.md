# tinkhireeva/so101_pick_place_yellow_objects_10_locations

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [tinkhireeva/so101_pick_place_yellow_objects_10_locations](https://huggingface.co/datasets/tinkhireeva/so101_pick_place_yellow_objects_10_locations), revision `07a0c9b896f0ffadd5b9ec8057036f5bc6c4159a` |
| Author | [tinkhireeva](https://huggingface.co/tinkhireeva) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 150 episodes, 88,093 frames |
| Rank | 240 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07386 of LeRobot's calibrated scale (a range of 2708 counts) | 1437 | 1487 |
| shoulder lift | 0.08414 of LeRobot's calibrated scale (a range of 2377 counts) | 1958 | 1971 |
| elbow flex | 0.08953 of LeRobot's calibrated scale (a range of 2234 counts) | 1835 | 1929 |
| wrist flex | 0.08450 of LeRobot's calibrated scale (a range of 2367 counts) | 1928 | 1941 |
| wrist roll | 0.05207 of LeRobot's calibrated scale (a range of 3841 counts) | 790 | 826 |
| gripper | 0.06882 of LeRobot's calibrated scale (a range of 1453 counts) | 841 | 768 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (41,265 frames); every frame then read as it would arrive. The held-out half (46,828 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 48 | 69 | 2 |
| shoulder lift | 58 | 93 | 1 |
| elbow flex | 50 | 92 | 3 |
| wrist flex | 38 | 75 | 38 |
| wrist roll | 31 | 55 | 8 |
| gripper | 21 | 110 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 144.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.848 | 1.226 | 1.649 | 1.505 |
| shoulder lift | 2.240 | 3.424 | 5.226 | 4.875 |
| elbow flex | 2.735 | 3.641 | 5.532 | 5.279 |
| wrist flex | 1.944 | 3.130 | 4.294 | 3.853 |
| wrist roll | 0.523 | 0.525 | 0.635 | 0.615 |
| gripper | 6.529 | 6.772 | 6.969 | 6.797 |
| five body joints | 1.858 | 2.703 | 3.986 | 3.717 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
