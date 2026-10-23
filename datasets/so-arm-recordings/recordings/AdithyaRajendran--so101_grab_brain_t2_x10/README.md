# AdithyaRajendran/so101_grab_brain_t2_x10

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [AdithyaRajendran/so101_grab_brain_t2_x10](https://huggingface.co/datasets/AdithyaRajendran/so101_grab_brain_t2_x10), revision `23970dd8ea236f10ca63e52e76855b56bef9bb5f` |
| Author | Adithya Rajendran ([AdithyaRajendran](https://huggingface.co/AdithyaRajendran)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 2,410 episodes, 1,008,320 frames |
| Rank | 9 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07524 of LeRobot's calibrated scale (a range of 2658 counts) | 1352 | 1362 |
| shoulder lift | 0.08500 of LeRobot's calibrated scale (a range of 2353 counts) | 1868 | 1901 |
| elbow flex | 0.09174 of LeRobot's calibrated scale (a range of 2180 counts) | 1277 | 1328 |
| wrist flex | 0.08811 of LeRobot's calibrated scale (a range of 2270 counts) | 1835 | 1848 |
| wrist roll | 0.05204 of LeRobot's calibrated scale (a range of 3843 counts) | 586 | 593 |
| gripper | 0.06854 of LeRobot's calibrated scale (a range of 1459 counts) | 641 | 566 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (504,160 frames); every frame then read as it would arrive. The held-out half (504,160 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 182 | 210 | 0 |
| shoulder lift | 144 | 257 | 0 |
| elbow flex | 192 | 238 | 0 |
| wrist flex | 28 | 320 | 0 |
| wrist roll | 131 | 144 | 0 |
| gripper | 204 | 241 | 0 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 22.2 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.744 | 0.907 | 1.914 | 1.317 |
| shoulder lift | 2.196 | 2.607 | 5.272 | 3.251 |
| elbow flex | 1.566 | 2.003 | 5.321 | 3.726 |
| wrist flex | 1.005 | 1.388 | 4.180 | 2.518 |
| wrist roll | 0.824 | 1.020 | 1.679 | 1.395 |
| gripper | 2.300 | 2.078 | 3.715 | 2.559 |
| five body joints | 1.379 | 1.708 | 4.001 | 2.626 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
