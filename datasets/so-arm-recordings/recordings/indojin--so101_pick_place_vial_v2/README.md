# indojin/so101_pick_place_vial_v2

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [indojin/so101_pick_place_vial_v2](https://huggingface.co/datasets/indojin/so101_pick_place_vial_v2), revision `eb38ac4d76ffd409ef73602aef01a75d2df37857` |
| Author | [indojin](https://huggingface.co/indojin) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 75,000 frames |
| Rank | 289 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1046 | 1060 |
| shoulder lift | 360/4,095 degree (degrees) | 788 | 835 |
| elbow flex | 360/4,095 degree (degrees) | 975 | 1031 |
| wrist flex | 360/4,095 degree (degrees) | 586 | 587 |
| wrist roll | 360/4,095 degree (degrees) | 288 | 316 |
| gripper | 0.06711 of LeRobot's calibrated scale (a range of 1490 counts) | 844 | 726 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (37,500 frames); every frame then read as it would arrive. The held-out half (37,500 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 23 | 90 | 35 |
| shoulder lift | 35 | 87 | 24 |
| elbow flex | 45 | 87 | 79 |
| wrist flex | 36 | 65 | 38 |
| wrist roll | 20 | 43 | 7 |
| gripper | 16 | 92 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 55.6 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.675 | 1.420 | 2.550 | 1.864 |
| shoulder lift | 1.417 | 2.276 | 4.673 | 4.129 |
| elbow flex | 0.932 | 2.046 | 3.775 | 2.893 |
| wrist flex | 0.726 | 1.157 | 2.126 | 1.622 |
| wrist roll | 0.257 | 0.256 | 0.470 | 0.466 |
| gripper | 3.663 | 4.191 | 6.065 | 5.342 |
| five body joints | 0.885 | 1.599 | 3.077 | 2.519 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
