# HCIS-Lab/soarm101-feeding-nuts

A public recording of a real SO-family robot arm, read by the Senua AI Robotics Core: the engine's reading of every joint, every frame.

| Source | Value |
|---|---|
| Dataset | [HCIS-Lab/soarm101-feeding-nuts](https://huggingface.co/datasets/HCIS-Lab/soarm101-feeding-nuts), revision `1dbb94e8fcb9b3bd5f38b410aa3d00ed0e45ae98` |
| Author | [HCIS-Lab](https://huggingface.co/HCIS-Lab) |
| Licence | apache-2.0 (as the dataset's card states it, at the revision read) |
| Robot type | `so101_follower` |
| Format | LeRobot v3.0, 30 frames a second |
| Recording | 80 episodes, 74,355 frames |
| Rank | 297 of the selection (one dataset an author, the largest by frames) |

## Servo counts

Each joint's recorded values, measured and commanded, lie on its servo's count lattice, one count apart (`lattice.tsv`): the recording's unit for one count, and the distinct positions seen:

| Joint | One count, in the recording's unit | Measured: distinct positions | Commanded: distinct positions |
|---|---|---|---|
| shoulder pan | 0.09940 of LeRobot's calibrated scale (a range of 2012 counts) | 981 | 924 |
| shoulder lift | 0.08446 of LeRobot's calibrated scale (a range of 2368 counts) | 1357 | 1378 |
| elbow flex | 0.09042 of LeRobot's calibrated scale (a range of 2212 counts) | 1774 | 1783 |
| wrist flex | 0.08532 of LeRobot's calibrated scale (a range of 2344 counts) | 2236 | 2194 |
| wrist roll | 0.05227 of LeRobot's calibrated scale (a range of 3826 counts) | 1308 | 1326 |
| gripper | 0.06553 of LeRobot's calibrated scale (a range of 1526 counts) | 1444 | 1199 |

## The engine's reading

Each joint's machine built once from the first half of the episodes (35,950 frames); every frame then read as it would arrive. The held-out half (38,405 frames):

| Joint | Causal states | Distinct changes | Held-out frames unseen |
|---|---|---|---|
| shoulder pan | 34 | 105 | 0 |
| shoulder lift | 38 | 131 | 6 |
| elbow flex | 40 | 158 | 21 |
| wrist flex | 19 | 159 | 26 |
| wrist roll | 21 | 116 | 49 |
| gripper | 18 | 165 | 4 |

## The simulator against this arm

The simulator's runs on this arm had not finished by the run's cutoff (amendment 17); they are still running, and are added when they finish.

## Files

`source.json` (the source, pinned), `lattice.tsv`, `streams/` (counts from each joint's lowest measured value: episode, frame, six measured, six goals), `states/` (every frame's causal state, each joint), `reading/` (the engine's reports).

The recording is the author's, under apache-2.0; the derived files here are under the same terms for the recording's content and CC BY 4.0 for Senua AI's outputs (see the collection's NOTICE.md).
