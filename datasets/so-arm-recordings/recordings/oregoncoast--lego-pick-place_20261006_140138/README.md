# oregoncoast/lego-pick-place_20261006_140138

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [oregoncoast/lego-pick-place_20261006_140138](https://huggingface.co/datasets/oregoncoast/lego-pick-place_20261006_140138), revision `773e57933975a5ca7d876ec456fd331a616bb5a3` |
| Author | [oregoncoast](https://huggingface.co/oregoncoast) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 150 episodes, 85,678 frames |
| Rank | 249 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 360/4,095 degree (degrees) | 739 | 753 |
| shoulder lift | 360/4,095 degree (degrees) | 1998 | 2009 |
| elbow flex | 360/4,095 degree (degrees) | 2075 | 2113 |
| wrist flex | 360/4,095 degree (degrees) | 543 | 557 |
| wrist roll | 360/4,095 degree (degrees) | 898 | 911 |
| gripper | 0.06649 of LeRobot's calibrated scale (a range of 1504 counts) | 869 | 731 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (42,495 frames); every frame then read as it would arrive. The held-out half (43,183 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 51 | 54 | 1 |
| shoulder lift | 71 | 92 | 1 |
| elbow flex | 56 | 93 | 2 |
| wrist flex | 40 | 66 | 5 |
| wrist roll | 38 | 55 | 1 |
| gripper | 13 | 116 | 13 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
