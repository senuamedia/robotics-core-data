# dadm022/lerobot_rgb_vla

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [dadm022/lerobot_rgb_vla](https://huggingface.co/datasets/dadm022/lerobot_rgb_vla), revision `cd609965081e01cdb8de3304e102d41e399d8daa` |
| Author | IT Determ ([dadm022](https://huggingface.co/dadm022)) |
| Licence | mit (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 2,318 episodes, 2,487,292 frames |
| Rank | 4 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 1692 | 1821 |
| shoulder lift | 360/4,095 degree (degrees) | 2187 | 2201 |
| elbow flex | 360/4,095 degree (degrees) | 2190 | 2208 |
| wrist flex | 360/4,095 degree (degrees) | 2295 | 2313 |
| wrist roll | 360/4,095 degree (degrees) | 2844 | 2860 |
| gripper | 0.06684 of LeRobot's calibrated scale (a range of 1496 counts) | 1460 | 1180 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (1,451,477 frames); every frame then read as it would arrive. The held-out half (1,035,815 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 94 | 357 | 6 |
| shoulder lift | 147 | 261 | 29 |
| elbow flex | 174 | 220 | 1 |
| wrist flex | 159 | 305 | 4 |
| wrist roll | 135 | 306 | 30 |
| gripper | 94 | 209 | 33 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under mit; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
