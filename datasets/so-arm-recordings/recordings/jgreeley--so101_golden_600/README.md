# jgreeley/so101_golden_600

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [jgreeley/so101_golden_600](https://huggingface.co/datasets/jgreeley/so101_golden_600), revision `cd1ba4b593bf397421fadb147eb1ef3b8078634a` |
| Author | Jeanine Greeley ([jgreeley](https://huggingface.co/jgreeley)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 518 episodes, 302,322 frames |
| Rank | 41 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07342 of LeRobot's calibrated scale (a range of 2724 counts) | 1668 | 1558 |
| shoulder lift | 0.08368 of LeRobot's calibrated scale (a range of 2390 counts) | 2126 | 2105 |
| elbow flex | 0.09070 of LeRobot's calibrated scale (a range of 2205 counts) | 2175 | 2200 |
| wrist flex | 0.08518 of LeRobot's calibrated scale (a range of 2348 counts) | 905 | 927 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 1170 | 1273 |
| gripper | 0.06729 of LeRobot's calibrated scale (a range of 1486 counts) | 1338 | 1056 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (167,718 frames); every frame then read as it would arrive. The held-out half (135,355 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 406 | 686 | 2 |
| shoulder lift | 671 | 921 | 9 |
| elbow flex | 545 | 887 | 2 |
| wrist flex | 294 | 662 | 0 |
| wrist roll | 328 | 480 | 2 |
| gripper | 226 | 456 | 3 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.240 | 0.900 | 1.867 | 1.317 |
| shoulder lift | 1.345 | 2.365 | 4.075 | 3.206 |
| elbow flex | 1.565 | 2.409 | 3.717 | 2.873 |
| wrist flex | 1.281 | 1.369 | 1.450 | 1.040 |
| wrist roll | 0.249 | 0.574 | 1.229 | 0.901 |
| gripper | 5.545 | 5.820 | 9.261 | 8.462 |
| five body joints | 1.096 | 1.697 | 2.739 | 2.104 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
