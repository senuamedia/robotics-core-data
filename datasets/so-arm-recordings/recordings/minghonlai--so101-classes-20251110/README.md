# minghonlai/so101-classes-20251110

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [minghonlai/so101-classes-20251110](https://huggingface.co/datasets/minghonlai/so101-classes-20251110), revision `e155b5c6be29589fb903b359cb01236e106ed5a8` |
| Author | [minghonlai](https://huggingface.co/minghonlai) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 200 episodes, 85,664 frames |
| Rank | 250 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07416 of LeRobot's calibrated scale (a range of 2697 counts) | 1238 | 1259 |
| shoulder lift | 0.08418 of LeRobot's calibrated scale (a range of 2376 counts) | 1965 | 1975 |
| elbow flex | 0.08949 of LeRobot's calibrated scale (a range of 2235 counts) | 1861 | 1899 |
| wrist flex | 0.08532 of LeRobot's calibrated scale (a range of 2344 counts) | 1432 | 1449 |
| wrist roll | 0.05212 of LeRobot's calibrated scale (a range of 3837 counts) | 2458 | 2488 |
| gripper | 0.06460 of LeRobot's calibrated scale (a range of 1548 counts) | 632 | 550 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (45,750 frames); every frame then read as it would arrive. The held-out half (39,914 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 31 | 95 | 38 |
| shoulder lift | 42 | 158 | 42 |
| elbow flex | 34 | 175 | 20 |
| wrist flex | 45 | 144 | 10 |
| wrist roll | 26 | 147 | 91 |
| gripper | 9 | 130 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.315 | 0.522 | 1.463 | 0.910 |
| shoulder lift | 2.133 | 3.154 | 5.101 | 3.529 |
| elbow flex | 1.383 | 2.604 | 5.338 | 3.805 |
| wrist flex | 0.486 | 0.734 | 1.995 | 1.234 |
| wrist roll | 0.405 | 0.870 | 2.387 | 1.451 |
| gripper | 4.273 | 4.288 | 4.902 | 4.504 |
| five body joints | 1.179 | 1.912 | 3.642 | 2.505 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
