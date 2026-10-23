# Samisaliveagain/so101_wm

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [Samisaliveagain/so101_wm](https://huggingface.co/datasets/Samisaliveagain/so101_wm), revision `6de54268d238ac883870b136fd19b39d5591c8de` |
| Author | Samyak Jain ([Samisaliveagain](https://huggingface.co/Samisaliveagain)) |
| Licence | mit (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 177 episodes, 355,884 frames |
| Rank | 36 of the selection (one dataset an author, the largest by frames) |

Its author asks it to be cited as:

```bibtex
@misc{so101wm2026,
  title  = {SO-101 Dual-View Teleoperation Dataset for World Models},
  author = {Samisaliveagain and Shubham},
  year   = {2026},
  url    = {https://huggingface.co/datasets/Samisaliveagain/so101_wm}
}
```

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 2229 | 2257 |
| shoulder lift | 360/4,095 degree (degrees) | 2305 | 2508 |
| elbow flex | 360/4,095 degree (degrees) | 2227 | 2217 |
| wrist flex | 360/4,095 degree (degrees) | 2393 | 2461 |
| wrist roll | 360/4,095 degree (degrees) | 3658 | 3817 |
| gripper | 0.06854 of LeRobot's calibrated scale (a range of 1459 counts) | 1438 | 1093 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (58,063 frames); every frame then read as it would arrive. The held-out half (297,821 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 26 | 161 | 57 |
| shoulder lift | 37 | 219 | 21 |
| elbow flex | 29 | 205 | 88 |
| wrist flex | 46 | 153 | 1202 |
| wrist roll | 59 | 157 | 1739 |
| gripper | 19 | 132 | 33 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 72.2 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.474 | 0.453 | 1.145 | 0.856 |
| shoulder lift | 2.989 | 4.195 | 6.774 | 5.913 |
| elbow flex | 1.676 | 3.361 | 6.029 | 5.025 |
| wrist flex | 1.998 | 2.050 | 2.140 | 1.527 |
| wrist roll | 0.692 | 0.586 | 1.415 | 1.040 |
| gripper | 11.721 | 11.895 | 13.379 | 12.946 |
| five body joints | 1.813 | 2.594 | 4.245 | 3.587 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under mit; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
