# access-robotics-team/so101_pp_stationery_5obj_color_select_v2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [access-robotics-team/so101_pp_stationery_5obj_color_select_v2](https://huggingface.co/datasets/access-robotics-team/so101_pp_stationery_5obj_color_select_v2), revision `440255981acd1b130b6483f8bab15890aff4172b` |
| Author | ACCESS Robotics Project ([access-robotics-team](https://huggingface.co/access-robotics-team)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 356 episodes, 274,615 frames |
| Rank | 45 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1249 | 1250 |
| shoulder lift | 360/4,095 degree (degrees) | 1652 | 1667 |
| elbow flex | 360/4,095 degree (degrees) | 1461 | 1538 |
| wrist flex | 360/4,095 degree (degrees) | 704 | 857 |
| wrist roll | 360/4,095 degree (degrees) | 1747 | 1773 |
| gripper | 0.06693 of LeRobot's calibrated scale (a range of 1494 counts) | 562 | 526 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (137,593 frames); every frame then read as it would arrive. The held-out half (137,022 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 56 | 64 | 15 |
| shoulder lift | 56 | 75 | 12 |
| elbow flex | 64 | 70 | 22 |
| wrist flex | 64 | 73 | 0 |
| wrist roll | 75 | 81 | 10 |
| gripper | 39 | 85 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.321 | 0.831 | 1.757 | 1.248 |
| shoulder lift | 1.696 | 2.322 | 3.998 | 3.367 |
| elbow flex | 1.049 | 1.869 | 3.709 | 3.152 |
| wrist flex | 3.328 | 3.516 | 4.019 | 3.693 |
| wrist roll | 0.467 | 1.219 | 2.439 | 1.725 |
| gripper | 4.028 | 4.071 | 4.108 | 3.983 |
| five body joints | 1.753 | 2.164 | 3.314 | 2.808 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
