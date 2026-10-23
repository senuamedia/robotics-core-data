# TomasAnderegg/so101_pill_sorting_v3

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [TomasAnderegg/so101_pill_sorting_v3](https://huggingface.co/datasets/TomasAnderegg/so101_pill_sorting_v3), revision `1d5758b230b42598449cf3731f745482d560b03b` |
| Author | Tomas ([TomasAnderegg](https://huggingface.co/TomasAnderegg)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 300 episodes, 242,137 frames |
| Rank | 56 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 876 | 886 |
| shoulder lift | 360/4,095 degree (degrees) | 2083 | 2098 |
| elbow flex | 360/4,095 degree (degrees) | 2094 | 2131 |
| wrist flex | 360/4,095 degree (degrees) | 933 | 969 |
| wrist roll | 360/4,095 degree (degrees) | 2072 | 2074 |
| gripper | 0.06605 of LeRobot's calibrated scale (a range of 1514 counts) | 484 | 413 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (139,444 frames); every frame then read as it would arrive. The held-out half (102,693 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 37 | 159 | 6 |
| shoulder lift | 83 | 164 | 32 |
| elbow flex | 55 | 167 | 8 |
| wrist flex | 62 | 164 | 9 |
| wrist roll | 39 | 230 | 1 |
| gripper | 23 | 119 | 1 |

## The simulator against this arm

A sealed arm, set against the simulator once, with its final model (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 27.8 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.265 | 0.264 | 0.742 | 0.535 |
| shoulder lift | 2.030 | 2.228 | 1.966 | 1.568 |
| elbow flex | 0.780 | 0.791 | 2.638 | 2.180 |
| wrist flex | 0.558 | 0.720 | 1.524 | 1.135 |
| wrist roll | 0.884 | 1.631 | 3.330 | 2.249 |
| gripper | 2.191 | 2.205 | 2.322 | 2.239 |
| five body joints | 1.085 | 1.329 | 2.226 | 1.663 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
