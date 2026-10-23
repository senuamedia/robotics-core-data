# yutofujita/so101-boccia-center-only

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [yutofujita/so101-boccia-center-only](https://huggingface.co/datasets/yutofujita/so101-boccia-center-only), revision `9a6bfb650042e2c486ea3335d50cebd02c071e78` |
| Author | [yutofujita](https://huggingface.co/yutofujita) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 175 episodes, 78,360 frames |
| Rank | 274 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07166 of LeRobot's calibrated scale (a range of 2791 counts) | 775 | 787 |
| shoulder lift | 0.08344 of LeRobot's calibrated scale (a range of 2397 counts) | 1919 | 1923 |
| elbow flex | 0.09017 of LeRobot's calibrated scale (a range of 2218 counts) | 2116 | 2173 |
| wrist flex | 0.08857 of LeRobot's calibrated scale (a range of 2258 counts) | 1102 | 1228 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 366 | 384 |
| gripper | 0.06680 of LeRobot's calibrated scale (a range of 1497 counts) | 1118 | 922 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (39,302 frames); every frame then read as it would arrive. The held-out half (39,954 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 163 | 212 | 326 |
| shoulder lift | 192 | 328 | 336 |
| elbow flex | 310 | 548 | 485 |
| wrist flex | 228 | 416 | 348 |
| wrist roll | 53 | 55 | 703 |
| gripper | 261 | 422 | 367 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 38.9 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.288 | 0.231 | 0.565 | 0.409 |
| shoulder lift | 1.509 | 2.362 | 4.149 | 2.904 |
| elbow flex | 1.283 | 1.929 | 3.779 | 2.772 |
| wrist flex | 0.679 | 0.943 | 2.161 | 1.423 |
| wrist roll | 0.116 | 0.116 | 0.299 | 0.296 |
| gripper | 1.110 | 1.452 | 3.244 | 2.531 |
| five body joints | 0.946 | 1.431 | 2.704 | 1.917 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
