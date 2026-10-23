# Servo recordings: real SO-100 and SO-101 arms, read by the Senua AI engine

Two public recordings of real, low-cost robot arms (an SO-101 and an SO-100, each driven by its six stock Feetech
STS3215 servos), made by others and published under the Apache License 2.0 [LRso101] [LRso100], read by the Senua AI
Robotics Core's causation engine: every joint, every frame, as it would arrive. The engine turns each joint's own motion
into causal states with no labels, no training run, no model of any kind and no GPU, on one core of an ordinary
processor. This release holds the recordings as converted, every frame's causal state for every joint, and the
reading's reports on two kinds of processor [P1]. Sources are cited by the keys in `REFERENCES.md`.

| Folder or file | What it holds |
|---|---|
| `METHODS.md` | How the recordings were converted and read, and what was checked |
| `REFERENCES.md` | Every source cited in this dataset's documents |
| `streams/` | The recordings as converted: each joint's measured and commanded position, every frame |
| `states/` | The engine's reading: every frame's causal state for every joint |
| `runs/` | The readings' own reports, on an ARM processor (`armb`) and an x86 processor (`x86b`): each joint's machine and the unseen share |
| `provenance/` | What the reading used: processors, and no GPU or model of any kind |
| `checks/` | The checks that the two processors' states are the same bytes and that the streams are their sources' |
| `SHA256SUMS` | A checksum of every file in this folder |

## What it shows

Measured on 25 October 2026 [runs] [checks], each against a bar registered before any of the data was read [P1]:

| Check | Result |
|---|---|
| The same reading on both processors | Every frame's six states are the same bytes on ARM (AWS Graviton3) and on x86 (Intel Xeon), for both recordings |
| A reading, not a copy | Every joint's machine has more than one causal state and fewer than its distinct changes (shoulder pan 53 of 294, shoulder lift 37 of 326, elbow 66 of 239, wrist flex 34 of 214, wrist roll 58 of 241, gripper 2 of 262) |
| Another arm | Read through the SO-101's machines, the SO-100 recording (another arm, recorded in degrees rather than LeRobot's calibrated scale) leaves 17 % to 48 % of its frames unseen, joint by joint: a machine reads the arm and the unit it was built from |

## What it does not show

It does not show that any causal state means a grip, a contact or a load; that is the next part of the same test, on
the recordings' videos annotated without sight of the engine's output, added to this dataset when it is done [P1]. It
gives no force, friction or mass: the recordings hold no servo load or current, so nothing here could check one. Those
come from the Robotics Core's own arm, recorded with known loads.

## How the numbers are written

| File | Values |
|---|---|
| `streams/*.txt` | One line a frame: the episode, the frame, the six measured positions, the six commanded positions, each in whole hundredths of the recording's own unit (the SO-101: LeRobot's calibrated scale, -100 to 100; the SO-100: degrees), in the recording's joint order (shoulder pan, shoulder lift, elbow, wrist flex, wrist roll, gripper) |
| `states/*.states` | One line a frame: the episode, the frame's line number, and the six joints' causal states (an internal Senua AI representation; -1 is unseen) |
| `runs/*/*.txt` | Each joint's causal states and distinct changes, and the unseen frames |

## Licence and how to cite

The converted recordings in `streams/` are derived from [LRso101] and [LRso100], copyright their authors and licensed
under the Apache License, Version 2.0 (`NOTICE` and `LICENSES/` at the repository's root). Everything else here, the
engine's reading and its measurements, is open data under the Creative Commons Attribution 4.0 International licence
[CCBY4]. Cite it as: Senua AI (2026), *Servo recordings: real SO-100 and SO-101 arms, read by the Senua AI engine*, senuamedia/robotics-core-data, `datasets/servo-recordings`; and credit the recordings to their authors.
