# funXedu/so101_lego_brick

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [funXedu/so101_lego_brick](https://huggingface.co/datasets/funXedu/so101_lego_brick), revision `8353c0544c1ffbf8782f88c44b24575f7405bd75` |
| Author | [funXedu](https://huggingface.co/funXedu) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 86 episodes, 94,192 frames |
| Rank | 214 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.08432 of LeRobot's calibrated scale (a range of 2372 counts) | 1421 | 1633 |
| shoulder lift | 0.08382 of LeRobot's calibrated scale (a range of 2386 counts) | 1939 | 1953 |
| elbow flex | 0.09046 of LeRobot's calibrated scale (a range of 2211 counts) | 1790 | 1818 |
| wrist flex | 0.08591 of LeRobot's calibrated scale (a range of 2328 counts) | 813 | 842 |
| wrist roll | 0.05231 of LeRobot's calibrated scale (a range of 3823 counts) | 2022 | 2086 |
| gripper | 0.06821 of LeRobot's calibrated scale (a range of 1466 counts) | 584 | 537 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (47,433 frames); every frame then read as it would arrive. The held-out half (46,759 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 14 | 111 | 3 |
| shoulder lift | 23 | 184 | 17 |
| elbow flex | 19 | 163 | 50 |
| wrist flex | 10 | 133 | 25 |
| wrist roll | 10 | 148 | 33 |
| gripper | 6 | 200 | 4 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.621 | 0.367 | 0.564 | 0.303 |
| shoulder lift | 2.445 | 1.292 | 2.785 | 1.204 |
| elbow flex | 2.200 | 1.422 | 2.331 | 1.174 |
| wrist flex | 1.536 | 1.061 | 1.589 | 0.959 |
| wrist roll | 0.867 | 0.325 | 0.951 | 0.394 |
| gripper | 2.959 | 2.727 | 2.670 | 2.457 |
| five body joints | 1.691 | 1.006 | 1.840 | 0.893 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
