# Methods: touch on a second arm

## The data

NAD's OpenManipulator-X part (revision 88838bce, MIT): a ROBOTIS OpenManipulator-X follower, five Dynamixel XM430-W350
servos at their default position gain, logged at about 58.8 Hz while a person moves a leader arm. Its `weight` tasks:
lift and hold with nothing or 200, 300 or 400 g; pick and place with nothing or 200 to 500 g; standard calibration
weights; 8 training, 1 validation and 1 test recording each. NAD labels each frame held (its force label set) or not.

- **Identification:** the empty recordings' training 001 to 004 (8). Nothing else was read to identify.
- **Sealed:** every task's training 005 to 008 (36), read only after the test was registered.

Each recording converted to the servo's counts (4,096 a turn) by the Robotics Core's one converter, from NAD's own
description of its columns: each frame's measured position at the simulator's tick, and the command in force at each
tick.

## The reading

The gripper's residual at a frame: its measured count less the command in force (the frame before's). The band: the
widest residual of the gripper at rest (not moved since the frame before) over the 8 identification recordings, 8
counts. Touch begins at a frame where the gripper is at rest and its residual exceeds the band, and lasts while it
does.

## The test (registered before the sealed recordings were read)

The arm's touch HOLDS if (i) no frame of the 8 sealed empty recordings is in touch, and (ii) over every frame of the 20
sealed pick-and-place recordings, the share whose touch agrees with NAD's held label is above the share of the more
common label. Both held: (i) 0 frames of 6,706; (ii) 90.2 % against 65.4 %.
