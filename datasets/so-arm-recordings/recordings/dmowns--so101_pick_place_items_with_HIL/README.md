# dmowns/so101_pick_place_items_with_HIL

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame, and the Robotics Core's simulator predicting the real follower from its commands alone.

| Source | Value |
|---|---|
| Dataset | [dmowns/so101_pick_place_items_with_HIL](https://huggingface.co/datasets/dmowns/so101_pick_place_items_with_HIL), revision `60bda8be5d0cae1c02d60e105a7e454c9a3e916b` |
| Author | Dimitar Manovski ([dmowns](https://huggingface.co/dmowns)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 473 episodes, 259,167 frames |
| Rank | 49 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1480 | 1574 |
| shoulder lift | 360/4,095 degree (degrees) | 1928 | 1920 |
| elbow flex | 360/4,095 degree (degrees) | 1911 | 1985 |
| wrist flex | 360/4,095 degree (degrees) | 952 | 1251 |
| wrist roll | 360/4,095 degree (degrees) | 2287 | 2282 |
| gripper | 0.07027 of LeRobot's calibrated scale (a range of 1423 counts) | 686 | 542 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (135,538 frames); every frame then read as it would arrive. The held-out half (123,629 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 70 | 102 | 16 |
| shoulder lift | 67 | 146 | 53 |
| elbow flex | 59 | 127 | 22 |
| wrist flex | 32 | 121 | 7 |
| wrist roll | 44 | 148 | 22 |
| gripper | 31 | 98 | 15 |

## The simulator against this arm

A development arm (amendment 8). 1 sample episode from the held-out half, 599 measured frames. World predicts the real follower from its commands alone, out of the box (the SO-101's published model at LeRobot's servo gain) and calibrated to this arm (its command latency, identified from the first half of its own recording: 83.3 ms). Each prediction's SHA-256 was committed to the record before it was compared with the measured arm. Root mean square gap in degrees; lower is better.

| Joint | World, calibrated | World, out of the box | B0, the command | B1, a frame later |
|---|---|---|---|---|
| shoulder pan | 0.343 | 0.573 | 1.191 | 0.885 |
| shoulder lift | 1.683 | 2.820 | 4.870 | 4.249 |
| elbow flex | 2.007 | 2.794 | 4.818 | 4.354 |
| wrist flex | 0.403 | 0.443 | 0.770 | 0.618 |
| wrist roll | 0.508 | 0.749 | 1.426 | 1.068 |
| gripper | 2.757 | 2.919 | 3.521 | 3.150 |
| five body joints | 1.216 | 1.835 | 3.193 | 2.804 |

B0: the follower exactly where it was last told; B1: a frame later still. The gripper's simulated jaw holds nothing, so where the real one stops on what it grips shows here as gap.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports), `calibration.txt` (the calibration identified), `sim/calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's per-tick output, with `PREDICTIONS.sha256`), `sim/measured/` (the measured and baseline tables), `gap/` (the gap reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
