# BlackWhite123/so101_redball

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [BlackWhite123/so101_redball](https://huggingface.co/datasets/BlackWhite123/so101_redball), revision `0cc574781a27d37f23d893bb07049d33b80d4526` |
| Author | [BlackWhite123](https://huggingface.co/BlackWhite123) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 51 episodes, 94,106 frames |
| Rank | 215 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1640 | 1670 |
| shoulder lift | 360/4,095 degree (degrees) | 1601 | 1637 |
| elbow flex | 360/4,095 degree (degrees) | 2005 | 2017 |
| wrist flex | 360/4,095 degree (degrees) | 1059 | 1150 |
| wrist roll | 360/4,095 degree (degrees) | 2124 | 2395 |
| gripper | 0.06720 of LeRobot's calibrated scale (a range of 1488 counts) | 879 | 772 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (49,521 frames); every frame then read as it would arrive. The held-out half (46,519 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 254 | 295 | 1 |
| shoulder lift | 165 | 238 | 2 |
| elbow flex | 288 | 386 | 6 |
| wrist flex | 60 | 132 | 14 |
| wrist roll | 99 | 171 | 8 |
| gripper | 89 | 211 | 6 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 33.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.568 | 0.795 | 1.718 | 1.124 |
| shoulder lift | 1.638 | 1.807 | 2.127 | 1.785 |
| elbow flex | 4.623 | 4.639 | 6.079 | 5.912 |
| wrist flex | 24.439 | 24.473 | 25.000 | 24.953 |
| wrist roll | 1.131 | 1.046 | 1.254 | 1.132 |
| gripper | 6.830 | 6.664 | 6.584 | 6.604 |
| five body joints | 11.161 | 11.184 | 11.583 | 11.518 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
