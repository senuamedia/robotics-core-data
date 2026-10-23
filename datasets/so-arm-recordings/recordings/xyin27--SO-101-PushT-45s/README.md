# xyin27/SO-101-PushT-45s

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [xyin27/SO-101-PushT-45s](https://huggingface.co/datasets/xyin27/SO-101-PushT-45s), revision `fb8e1e94e687b66d9a3e0338297091a216d8a9a8` |
| Author | Rain ([xyin27](https://huggingface.co/xyin27)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 230 episodes, 206,633 frames |
| Rank | 74 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 847 | 873 |
| shoulder lift | 360/4,095 degree (degrees) | 1908 | 1933 |
| elbow flex | 360/4,095 degree (degrees) | 1860 | 1908 |
| wrist flex | 360/4,095 degree (degrees) | 2228 | 2249 |
| wrist roll | 360/4,095 degree (degrees) | 131 | 163 |
| gripper | 0.06378 of LeRobot's calibrated scale (a range of 1568 counts) | 144 | 125 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (112,579 frames); every frame then read as it would arrive. The held-out half (102,067 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 45 | 67 | 20 |
| shoulder lift | 71 | 115 | 8 |
| elbow flex | 92 | 106 | 10 |
| wrist flex | 54 | 170 | 24 |
| wrist roll | 6 | 39 | 11 |
| gripper | 4 | 4 | 127 |

## The simulator against this arm

Not in the simulator test: this arm was recorded on another zero (a body joint's range is not centred inside the model's range; amendment 16), so it is the engine's reading only.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
