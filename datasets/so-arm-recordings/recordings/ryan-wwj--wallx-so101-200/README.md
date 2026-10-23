# ryan-wwj/wallx-so101-200

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [ryan-wwj/wallx-so101-200](https://huggingface.co/datasets/ryan-wwj/wallx-so101-200), revision `09fbfd5c7638dc474bf0a3bab3d2f1dc647d391d` |
| Author | [ryan-wwj](https://huggingface.co/ryan-wwj) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 201 episodes, 112,218 frames |
| Rank | 166 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07479 of LeRobot's calibrated scale (a range of 2674 counts) | 1318 | 1356 |
| shoulder lift | 0.08418 of LeRobot's calibrated scale (a range of 2376 counts) | 1624 | 1613 |
| elbow flex | 0.09074 of LeRobot's calibrated scale (a range of 2204 counts) | 1694 | 1719 |
| wrist flex | 0.08540 of LeRobot's calibrated scale (a range of 2342 counts) | 836 | 860 |
| wrist roll | 0.05253 of LeRobot's calibrated scale (a range of 3807 counts) | 694 | 700 |
| gripper | 0.06757 of LeRobot's calibrated scale (a range of 1480 counts) | 590 | 509 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (58,574 frames); every frame then read as it would arrive. The held-out half (53,644 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 50 | 70 | 18 |
| shoulder lift | 63 | 80 | 21 |
| elbow flex | 66 | 82 | 62 |
| wrist flex | 52 | 57 | 79 |
| wrist roll | 41 | 44 | 39 |
| gripper | 18 | 106 | 2 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.340 | 0.790 | 2.013 | 1.326 |
| shoulder lift | 0.888 | 1.402 | 3.235 | 2.419 |
| elbow flex | 0.540 | 1.416 | 3.720 | 2.813 |
| wrist flex | 4.173 | 4.192 | 1.572 | 1.037 |
| wrist roll | 0.234 | 0.225 | 0.923 | 0.573 |
| gripper | 6.259 | 6.308 | 6.631 | 6.396 |
| five body joints | 1.931 | 2.107 | 2.517 | 1.839 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
