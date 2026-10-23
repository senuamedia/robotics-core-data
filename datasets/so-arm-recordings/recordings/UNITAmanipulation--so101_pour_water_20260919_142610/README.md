# UNITAmanipulation/so101_pour_water_20260919_142610

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [UNITAmanipulation/so101_pour_water_20260919_142610](https://huggingface.co/datasets/UNITAmanipulation/so101_pour_water_20260919_142610), revision `7efc52c8b66ed32263230a20829b8213056ec8dc` |
| Author | unita_manipulation ([UNITAmanipulation](https://huggingface.co/UNITAmanipulation)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 100 episodes, 127,279 frames |
| Rank | 136 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 491 | 505 |
| shoulder lift | 360/4,095 degree (degrees) | 1794 | 1796 |
| elbow flex | 360/4,095 degree (degrees) | 1277 | 1347 |
| wrist flex | 360/4,095 degree (degrees) | 1395 | 1420 |
| wrist roll | 360/4,095 degree (degrees) | 1999 | 2014 |
| gripper | 0.05491 of LeRobot's calibrated scale (a range of 1821 counts) | 1748 | 1232 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (70,145 frames); every frame then read as it would arrive. The held-out half (57,134 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 46 | 55 | 21 |
| shoulder lift | 36 | 112 | 7 |
| elbow flex | 38 | 78 | 13 |
| wrist flex | 18 | 131 | 6 |
| wrist roll | 32 | 123 | 5 |
| gripper | 20 | 132 | 5 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
