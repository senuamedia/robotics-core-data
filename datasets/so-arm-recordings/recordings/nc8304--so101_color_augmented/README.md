# nc8304/so101_color_augmented

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [nc8304/so101_color_augmented](https://huggingface.co/datasets/nc8304/so101_color_augmented), revision `9531005e8aa8f3b4cf80607431b23685a3b5a927` |
| Author | Nicole ([nc8304](https://huggingface.co/nc8304)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Built from (as its card credits) | [nc8304/so101_combined](https://huggingface.co/datasets/nc8304/so101_combined), by Nicole (nc8304), apache-2.0 |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 3,200 episodes, 2,440,160 frames |
| Rank | 5 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 950 | 961 |
| shoulder lift | 360/4,095 degree (degrees) | 1836 | 1925 |
| elbow flex | 360/4,095 degree (degrees) | 2110 | 2143 |
| wrist flex | 360/4,095 degree (degrees) | 943 | 963 |
| wrist roll | 360/4,095 degree (degrees) | 2016 | 2020 |
| gripper | 0.06716 of LeRobot's calibrated scale (a range of 1489 counts) | 854 | 749 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (1,220,080 frames); every frame then read as it would arrive. The held-out half (1,220,080 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 70 | 126 | 0 |
| shoulder lift | 149 | 218 | 0 |
| elbow flex | 205 | 273 | 0 |
| wrist flex | 135 | 190 | 0 |
| wrist roll | 179 | 257 | 0 |
| gripper | 120 | 183 | 0 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 44.4 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.259 | 0.246 | 0.736 | 0.544 |
| shoulder lift | 6.826 | 6.930 | 6.739 | 6.529 |
| elbow flex | 0.859 | 1.559 | 3.621 | 2.752 |
| wrist flex | 1.335 | 1.328 | 1.011 | 0.655 |
| wrist roll | 0.341 | 0.700 | 1.626 | 1.053 |
| gripper | 0.037 | 0.037 | 0.160 | 0.160 |
| five body joints | 3.139 | 3.248 | 3.541 | 3.225 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
