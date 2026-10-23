# lanyard2400/soarm_pp_rgb_random_pose_red_target_600ep

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [lanyard2400/soarm_pp_rgb_random_pose_red_target_600ep](https://huggingface.co/datasets/lanyard2400/soarm_pp_rgb_random_pose_red_target_600ep), revision `1e0ffac80fc833c1fc6e4f02f2bf2afc6ff600ea` |
| Author | Hiroki Okuyama ([lanyard2400](https://huggingface.co/lanyard2400)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 600 episodes, 221,914 frames |
| Rank | 67 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.07391 of LeRobot's calibrated scale (a range of 2706 counts) | 937 | 954 |
| shoulder lift | 0.08514 of LeRobot's calibrated scale (a range of 2349 counts) | 1828 | 1832 |
| elbow flex | 0.09054 of LeRobot's calibrated scale (a range of 2209 counts) | 1736 | 1749 |
| wrist flex | 0.08711 of LeRobot's calibrated scale (a range of 2296 counts) | 825 | 855 |
| wrist roll | 0.05233 of LeRobot's calibrated scale (a range of 3822 counts) | 2345 | 2512 |
| gripper | 0.06725 of LeRobot's calibrated scale (a range of 1487 counts) | 914 | 799 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (107,962 frames); every frame then read as it would arrive. The held-out half (113,952 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 43 | 125 | 0 |
| shoulder lift | 42 | 154 | 1 |
| elbow flex | 38 | 147 | 2 |
| wrist flex | 67 | 105 | 2 |
| wrist roll | 29 | 171 | 16 |
| gripper | 13 | 156 | 3 |

## The simulator against this arm

A development arm (amendment 8). 2 sample episodes from the held-out half, 598 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.596 | 1.219 | 2.254 | 1.658 |
| shoulder lift | 2.609 | 3.891 | 5.412 | 3.919 |
| elbow flex | 0.904 | 2.616 | 5.577 | 4.293 |
| wrist flex | 0.499 | 0.664 | 1.150 | 0.904 |
| wrist roll | 0.573 | 1.150 | 2.086 | 1.542 |
| gripper | 6.795 | 6.924 | 8.974 | 8.169 |
| five body joints | 1.307 | 2.246 | 3.772 | 2.818 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
