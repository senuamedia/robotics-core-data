# endoard/grab_cup_2cam_v5

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [endoard/grab_cup_2cam_v5](https://huggingface.co/datasets/endoard/grab_cup_2cam_v5), revision `6c2ec967a8b440dcfaadd030243542922236c046` |
| Author | [endoard](https://huggingface.co/endoard) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 498 episodes, 78,771 frames |
| Rank | 272 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1482 | 1554 |
| shoulder lift | 360/4,095 degree (degrees) | 1818 | 1829 |
| elbow flex | 360/4,095 degree (degrees) | 1840 | 1844 |
| wrist flex | 360/4,095 degree (degrees) | 400 | 412 |
| wrist roll | 360/4,095 degree (degrees) | 365 | 398 |
| gripper | 0.03870 of LeRobot's calibrated scale (a range of 2584 counts) | 643 | 685 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (35,060 frames); every frame then read as it would arrive. The held-out half (43,711 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 24 | 102 | 13 |
| shoulder lift | 49 | 113 | 0 |
| elbow flex | 31 | 116 | 4 |
| wrist flex | 25 | 79 | 23 |
| wrist roll | 18 | 63 | 69 |
| gripper | 7 | 116 | 0 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 7 sample episodes from the held-out half, 593 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 66.7 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.496 | 0.562 | 0.892 | 0.770 |
| shoulder lift | 1.804 | 1.972 | 2.101 | 1.722 |
| elbow flex | 1.127 | 1.701 | 3.817 | 3.435 |
| wrist flex | 0.461 | 0.471 | 0.532 | 0.430 |
| wrist roll | 0.239 | 0.239 | 0.465 | 0.450 |
| gripper | 21.557 | 21.746 | 18.515 | 18.312 |
| five body joints | 1.003 | 1.214 | 2.013 | 1.774 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
