# zhenxuan/so100-put-apricot

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [zhenxuan/so100-put-apricot](https://huggingface.co/datasets/zhenxuan/so100-put-apricot), revision `8795a43916c509d188978ad0866c1cbb06c90e72` |
| Author | [zhenxuan](https://huggingface.co/zhenxuan) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 63 episodes, 96,108 frames |
| Rank | 207 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1486 | 1509 |
| shoulder lift | 360/4,095 degree (degrees) | 1861 | 1944 |
| elbow flex | 360/4,095 degree (degrees) | 1504 | 1546 |
| wrist flex | 360/4,095 degree (degrees) | 2144 | 2175 |
| wrist roll | 360/4,095 degree (degrees) | 1890 | 1914 |
| gripper | 0.07032 of LeRobot's calibrated scale (a range of 1422 counts) | 701 | 611 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (52,487 frames); every frame then read as it would arrive. The held-out half (43,621 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 61 | 76 | 12 |
| shoulder lift | 70 | 100 | 22 |
| elbow flex | 53 | 86 | 65 |
| wrist flex | 42 | 112 | 37 |
| wrist roll | 56 | 139 | 32 |
| gripper | 49 | 122 | 18 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.298 | 0.384 | 1.213 | 0.770 |
| shoulder lift | 3.128 | 3.233 | 3.879 | 3.342 |
| elbow flex | 1.116 | 1.427 | 2.545 | 1.800 |
| wrist flex | 0.415 | 0.808 | 2.203 | 1.491 |
| wrist roll | 0.361 | 1.071 | 2.516 | 1.618 |
| gripper | 7.226 | 7.254 | 7.454 | 7.281 |
| five body joints | 1.511 | 1.698 | 2.614 | 1.992 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
