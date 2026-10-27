# Methods

## The data

The SO-101 part of the Neural Actuation Dataset [NAD]: a real SO-101 follower arm with six stock STS3215 servos at
12.2 V, moved by a person through a leader arm, about 62.3 frames a second. Lift-and-hold ("go up and stay still") and
pick-and-place, each with no payload or a standard calibration weight of 200, 300, 400 or 500 g in the jaws, each
combination recorded 10 times (the authors' split: 8 training, 1 validation, 1 test). Per frame: each joint's measured
and commanded position and its servo telemetry, and `force_z`, the payload's weight while it is held.

- **Identification: the 8 empty training recordings 001 to 004** (4 of each task). Nothing else was read to identify.
- **Sealed: the training recordings 005 to 008.** No weight or touch had been read on them; the test was registered
  before any reading, and decided on them alone. This dataset holds the lift-and-hold recordings.
- The validation and test recordings were used to develop the method.

## The conversion and the simulator

Each recording's angles (NAD's radians from the middle of each servo's calibrated range, 4,095 to a turn) are turned
into the model's angles and the servo's counts, as the SO-101 recordings of `datasets/so-arm-recordings` are; the frame
period is NAD's own, 16.05 ms. World plays each recording from its first measured frame and its commands alone, with the
SO-101's published model [TRS] and the arm's command latency, twice: with nothing in the jaws, and with a 500 g point
mass at the jaws' centre, joined to the gripper by the parallel-axis theorem.

## The identification (the 8 empty recordings)

1. **The command latency**: the whole number of World steps for which World's run of each recording, shifted later by
   it, is nearest the measured follower (as in `datasets/so-arm-recordings-remeasured`): 2 steps, 10.7 ms.
2. **Each body joint's offset**: the mean of the measured follower less World's angle over the joint's lifting frames.
3. **The jaw's band**: the widest gap between the jaw's measured position and its command at any frame where the jaw
   was at rest: 13 counts.

## The reading

- A body joint **lifts** at a frame when its measured position moved since the frame before in the direction its servo
  pushes in World, and World's joint moves that way too: the servo doing work against its load.
- The jaw **holds something** from a frame where it rests further from its command than the band, for as long as it
  stays further.
- While it holds something, over every body joint's lifting frames since it took hold: d, the measured position less
  World's with nothing in the jaws (plus the offset), and u, World's position with 500 g less with nothing. The reading
  is m = 500 g x sum(d u) / sum(u u), the least-squares weight [Ljung1999]; 0 while nothing is held and before the
  first lift. The **end reading** is the reading at the last frame the jaw held something.

World's position here is its joint angle in counts (`q_nrad` x 4,096 / 2 pi), not its `encoder` column: the rigs also
carry World's calibration of the arm's encoders (an offset and a sag a joint, identified with the latency), which the
reading does not use. Exact whole-number arithmetic throughout; World's predictions hashed before any reading.

## The test (registered before any reading of the sealed recordings)

The mean absolute error of the end reading over the 16 sealed lift-and-hold recordings with a payload must be below the
error of the best guess made without feeling the weight: a constant, the median of their payloads (300 g, error 100 g).
**Result: 25 g. Holds.**
