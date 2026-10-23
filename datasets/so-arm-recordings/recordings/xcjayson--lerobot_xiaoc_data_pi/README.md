# xcjayson/lerobot_xiaoc_data_pi

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [xcjayson/lerobot_xiaoc_data_pi](https://huggingface.co/datasets/xcjayson/lerobot_xiaoc_data_pi), revision `863e3b581f21e0b4b053679d116ec188abc12506` |
| Author | [xcjayson](https://huggingface.co/xcjayson) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 98,800 frames |
| Rank | 204 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1808 | 1830 |
| shoulder lift | 360/4,095 degree (degrees) | 2043 | 2071 |
| elbow flex | 360/4,095 degree (degrees) | 2165 | 2198 |
| wrist flex | 360/4,095 degree (degrees) | 562 | 570 |
| wrist roll | 360/4,095 degree (degrees) | 1501 | 1551 |
| gripper | 0.06752 of LeRobot's calibrated scale (a range of 1481 counts) | 447 | 569 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (50,900 frames); every frame then read as it would arrive. The held-out half (47,900 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 51 | 102 | 16 |
| shoulder lift | 55 | 203 | 34 |
| elbow flex | 64 | 212 | 40 |
| wrist flex | 46 | 141 | 18 |
| wrist roll | 33 | 107 | 14 |
| gripper | 11 | 102 | 7 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.327 | 0.522 | 1.409 | 0.869 |
| shoulder lift | 2.866 | 3.821 | 8.039 | 7.080 |
| elbow flex | 2.268 | 3.030 | 5.743 | 4.781 |
| wrist flex | 3.613 | 3.618 | 1.179 | 0.819 |
| wrist roll | 0.275 | 0.338 | 0.994 | 0.637 |
| gripper | 7.833 | 7.833 | 7.790 | 7.761 |
| five body joints | 2.306 | 2.729 | 4.515 | 3.868 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
