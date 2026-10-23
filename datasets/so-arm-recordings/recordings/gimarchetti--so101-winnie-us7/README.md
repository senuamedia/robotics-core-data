# gimarchetti/so101-winnie-us7

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [gimarchetti/so101-winnie-us7](https://huggingface.co/datasets/gimarchetti/so101-winnie-us7), revision `a48be0b262e75b12ee8c672c47592d4f45c0f409` |
| Author | Giovanni Marchetti ([gimarchetti](https://huggingface.co/gimarchetti)) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 121 episodes, 139,436 frames |
| Rank | 124 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.11319 of LeRobot's calibrated scale (a range of 1767 counts) | 886 | 851 |
| shoulder lift | 0.08471 of LeRobot's calibrated scale (a range of 2361 counts) | 2002 | 2006 |
| elbow flex | 0.09103 of LeRobot's calibrated scale (a range of 2197 counts) | 1856 | 1905 |
| wrist flex | 0.08764 of LeRobot's calibrated scale (a range of 2282 counts) | 1550 | 1580 |
| wrist roll | 0.04884 of LeRobot's calibrated scale (a range of 4095 counts) | 321 | 316 |
| gripper | 0.06940 of LeRobot's calibrated scale (a range of 1441 counts) | 825 | 713 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (69,477 frames); every frame then read as it would arrive. The held-out half (69,959 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 43 | 93 | 5 |
| shoulder lift | 76 | 119 | 10 |
| elbow flex | 51 | 88 | 6 |
| wrist flex | 61 | 125 | 14 |
| wrist roll | 51 | 59 | 1 |
| gripper | 11 | 155 | 1 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
