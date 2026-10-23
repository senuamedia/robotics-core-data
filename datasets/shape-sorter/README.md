# Shape sorter: a Senua AI Robotics Core simulation, as data

A simulated SO-101 robot arm picks five shapes (a circle, a square, a rectangle, a triangle and a semicircle) from a
bench, carries each to a raised board, turns it to its cutout's angle, lowers it in and lets go. Each shape falls
through its cutout onto the bench beneath. The run is 48.4 seconds long. It was computed by the physics engine of the
Senua AI Robotics Core, and the film published with it ("Senua AI Robotics Core: the shape sorter") is drawn from its
frames [generation]. The film itself is not part of this repository; the data behind every frame and every number in it
is [checks: check_film.txt]. Sources are cited by the keys in `REFERENCES.md`.

| Folder or file | What it holds |
|---|---|
| `METHODS.md` | How the data was generated and checked, and the physics applied, with its peer-reviewed sources |
| `REFERENCES.md` | Every source cited in this dataset's documents |
| `rig/` | The input: the arm, its joints and servos, the five shapes, the board and the arm's goal timeline |
| `physics/` | The output: every part's position and orientation, and every joint's angle, rate, servo goal, torque and encoder count, every 6.25 ms |
| `commands/` | The command stream for the six servos at every 6.25 ms tick: as angles, as servo counts, and as the exact bytes for the servo bus |
| `realtime/` | Measurements of that command stream played in real time |
| `prediction/` | The sim-to-real forecast, published before the real arm is run: the gap we expect and how it will be measured |
| `film/` | The film's data: the World frame behind every film frame, and every number the film shows, with where it is here |
| `provenance/` | What the run used to compute: its computer, memory, and no GPU or model of any kind |
| `checks/` | The reports of the programs that checked every number here against the files, and the film against them |
| `SHA256SUMS` | A checksum of every file in this folder |

This dataset is open data under the Creative Commons Attribution 4.0 International licence [CCBY4]: anyone may use it for
any purpose, with attribution to Senua AI (see "Licence and how to cite" below).

## How the numbers are written

Every value in the generated files (`physics/`, `commands/`, `realtime/`, `film/`, `prediction/`) is a whole number in a
stated unit; only the rig (the input, `rig/sorter.rig`) states its values as decimals, in SI units:

| Unit | Meaning |
|---|---|
| `ns` | nanoseconds (10^-9 s) |
| `nm` | nanometres (10^-9 m) |
| `nrad` | nanoradians (10^-9 rad); `nrad_s` nanoradians per second |
| `e9` | billionths: a value of 1,000,000,000 is 1 |
| `nnm` | nanonewton-metres (10^-9 N m) |
| counts | servo encoder counts: 4,096 to one full turn |

Each value is rounded once, to nearest with ties to even [IEEE754], from the engine's own value, which by the engine's
design is exact (`METHODS.md`). Two separate runs of this rig gave the same bytes, all 7,745 frames [generation].

**World's frame.** Metres in the rig, nanometres in the data; z points up (gravity is the standard 9.80665 m/s^2 along
-z [BIPM2019]), and the ground plane z = 0 is the bench top the arm, the board and the shapes stand on.

**The joints**, in the order of every per-joint column (`_1` to `_6`), are the SO-101's six servos, ids 1 to 6 as the
SO-101 follower assigns them [LeRobot]:

| Column suffix | Servo id | Joint | Moves link |
|---|---|---|---|
| `_1` | 1 | shoulder_pan | 1 |
| `_2` | 2 | shoulder_lift | 2 |
| `_3` | 3 | elbow_flex | 3 |
| `_4` | 4 | wrist_flex | 4 |
| `_5` | 5 | wrist_roll | 5 |
| `_6` | 6 | gripper (the moving jaw) | 6 |

Each joint angle is measured from the zero of the SO-101's published model (`so101_new_calib.urdf` [SOARM100]) and is
positive in that model's sense of rotation.

**The shapes**, in the order of every per-shape column (`shape1` to `shape5`): 1 circle, 2 square, 3 rectangle,
4 triangle, 5 semicircle. They are picked in that order.

## `rig/sorter.rig`

The run's whole input, as the engine read it: the step (0.00625 s), gravity, the ground, each link of the arm (its
axis, size, mass, centre of mass, inertia and starting angle), each joint (its limits and friction), each joint's
encoder (4,096 counts a turn [rustypot]) and servo (stiffness 17.8 N m/rad, torque limit 3.35 N m, damping 0.60 N m
s/rad, rotor inertia 0.028 kg m^2 [SOARM100]), the grip, the five shapes and the board, the times the gripper grasps and
releases each shape, and the arm's goal timeline (the `goal` lines: a time, then the six joints' goal angles in radians;
between two goal lines the goals move linearly). The servo values and the joints' friction (0.052 N m) are the SO-101's
published ones [SOARM100]; they are a model of the real servo, to be measured on the arm (`prediction/`). The file is the
engine's input with its comment lines removed (the engine ignores them).

## `physics/`

### `physics/joints.csv`: the arm, one row per frame

7,745 rows, frame 0 (the start) to frame 7,744 (48.4 s). Columns:

| Column | Meaning |
|---|---|
| `frame` | The frame: 0 to 7,744 |
| `t_ns` | Its time from the start: `frame` x 6,250,000 ns |
| `refusals` | An internal Senua AI representation |
| `q_nrad_1` to `_6` | Each joint's angle (nrad) |
| `qd_nrad_s_1` to `_6` | Each joint's rate of turn (nrad per second) |
| `goal_nrad_1` to `_6` | Each servo's goal angle at this frame's time (nrad): the rig's goal timeline |
| `torque_nnm_1` to `_6` | The torque each servo applied over the step that ended at this frame (nN m); 0 at frame 0 |
| `encoder_1` to `_6` | Each joint's encoder reading: the servo's present position in counts from the model's zero (4,096 counts a turn [rustypot]). **This is the sim-to-real prediction** (see `prediction/`) |
| `grasps` | Shapes the gripper has taken hold of so far (its jaws closed on the shape) |
| `slips` | Times a held shape has slipped in the jaws so far (0 in every row of this run) |
| `releases` | Shapes the gripper has let go of so far |
| `held` | The shape the gripper holds now: 0 circle, 1 square, 2 rectangle, 3 triangle, 4 semicircle; -1 none |

### `physics/pose.csv`: every part, one row per frame

7,745 rows, the same frames as `joints.csv`. After `frame`, `t_ns` and `refusals` (as above), for each link 1 to 6:

| Columns | Meaning |
|---|---|
| `linkN_joint_x_nm`, `_y_nm`, `_z_nm` | The point of link N's joint, on its axis, in World's frame (nm) |
| `linkN_centre_x_nm`, `_y_nm`, `_z_nm` | Link N's centre of mass in World's frame (nm) |
| `linkN_qw_e9`, `_qx_e9`, `_qy_e9`, `_qz_e9` | Link N's orientation, a unit quaternion (w, x, y, z), in billionths |

and for each shape 1 to 5, `shapeN_centre_*_nm` (its reference point: the circle's, square's and rectangle's centre, the
triangle's centroid, the centre of the semicircle's flat face) and `shapeN_q*_e9` (its orientation, as above).

### `physics/world_pose_capture.txt`

The same frames as `pose.csv`, as the engine wrote them: a header line (`POSE`, then `kind`, an internal Senua AI
representation; `parts`, 11: the six links and five shapes; `step_ns`, 6,250,000; `steps`, 7,744), then one line of
space-separated whole numbers per frame, in `pose.csv`'s column order. The film was drawn from these frames [generation].

### `physics/clearance.csv`: each shape's clearance through its cutout

5 rows, one per shape (`shape` 1 to 5, in the order above). Measured on the frames of `pose.csv`: from the first frame
after the shape is let go, while any part of it is within the board's thickness, the smallest distance of the shape's
outline (both its faces, in its orientation at that frame) inside its cutout's walls. A negative value would mean it
passed through a wall. Columns: `at_release_um` (that distance at the first frame after the release, in micrometres),
`least_um` (the smallest over those frames), `frames_within_board` (how many frames that is), in whole micrometres
computed from the nanometre poses. The board and the cutouts it measures against are `rig/sorter_measure.txt`.

### `rig/sorter_measure.txt`

The board and the cutouts, as the clearance measure reads them (nm): the `board` line (the board's top and underside,
above the bench), then for each shape a `shape` line (its index from 0, its grasp and release times in ns, its
thickness, and how many outline and cutout points follow), its outline (`o`, points about its centre of mass in its own
frame) and its cutout (`c`, points on the board in World's frame).

## `commands/`

### `commands/goals.csv`: one row per 6.25 ms tick

7,745 rows, tick 0 to tick 7,744 (tick k is at k x 6.25 ms). Columns: `tick`, `t_ns`, `goal_nrad_1` to `_6` (each
servo's goal angle; the same values as `goal_nrad` in `physics/joints.csv`), and `count_1` to `_6` (the same goals as
STS3215 servo counts from the model's zero: the angle x 4,096 / 2 pi [rustypot], rounded to nearest, ties to even
[IEEE754]).

### `commands/sync_write_packets.bin`: the bytes for the servo bus

7,745 packets, one per tick, in order, each 26 bytes (201,370 bytes in all). Each is one STS "sync write" [DXL1;
rustypot] that sets all six servos' goal position at once:

| Bytes | Value | Meaning |
|---|---|---|
| 0-1 | `FF FF` | Header |
| 2 | `FE` | Broadcast id (every servo on the bus reads it) |
| 3 | `16` | Length: 22 bytes follow (instruction, parameters, checksum) |
| 4 | `83` | Instruction: sync write |
| 5 | `2A` | Start register: 42, the STS3215's goal position [rustypot] |
| 6 | `02` | Bytes per servo: 2 |
| 7-24 | | Six groups of three bytes: the servo id (1 to 6 [LeRobot]), then its goal in counts, little-endian, as sign and magnitude (bit 15 set for a negative count) [rustypot] |
| 25 | | Checksum: the complement of the low byte of the sum of bytes 2 to 24 [DXL1] |

### `commands/sync_write_packets.csv`

The same packets, one row per tick: `tick`, `t_ns`, `packet_hex`.

**Calibration.** The counts are from the model's zero, in the model's sense of rotation. A physical arm is calibrated
before the stream is played, so that each servo's zero is the model's zero and its positive direction the model's.

## `realtime/`

The command stream played on its schedule, one packet every 6.25 ms for 48.4 s, to a null device (no arm attached),
with each packet's timing measured; paced by absolute-deadline sleeps, with real-time scheduling, one processor and
locked memory where the operating system offers them [LinuxSched].

| File | Computer | Result |
|---|---|---|
| `playback_linux_realtime.csv` | Amazon EC2 c7i.2xlarge [AWSc7i], Linux, real-time scheduling | All 7,745 packets on time: the latest 14.2 us after its time, the mean 8.2 us, no tick missed [realtime; checks: check_realtime.txt] |
| `playback_macos.csv` | Apple MacBook Pro (MacBookPro16,2: Intel Core i5-1038NG7, 4 cores, 16 GB), macOS 26.6.2, no real-time scheduling | 42 ticks missed (the player woke a tick or more late), 147 packets not sent because a later goal was already due; the latest wake 71.9 ms late, the mean 1.0 ms [realtime; checks: check_realtime.txt] |

Columns: `tick` (the tick written), `late_ns` (how long after its time the player woke), `write_ns` (how long the write
took). On Linux every wake was within 50 us of its time [checks: check_realtime.txt]; the arm will be driven from Linux.

## `film/`: the film's data

The film ("Senua AI Robotics Core: the shape sorter", 2 minutes 18 seconds, 4,152 frames at 30 a second) is published
separately; this folder ties it to the data.

| File | What it holds |
|---|---|
| `film_frame_map.csv` | For every film frame (`film_frame`, 0 to 4,151), the World frame of the footage it shows (`world_frame_a`), and of a second picture during a crossfade (`world_frame_b`); -1 where the film shows no footage. 2,426 film frames show footage, 22 of them two pictures |
| `footage_clips.txt` | The footage the film is cut from, a line per clip: its index, its film frames, the World time it starts at (ms) and its speed (thousandths of real speed). Film frame i of a clip shows World frame floor((start + (i / 30) x speed) / 6.25 ms) |
| `footage_frame_map.csv` | Every footage clip frame's World frame (`clip`, `film_frame`, `world_frame`) by that rule |
| `film_data_scene.csv`, `film_figures.csv` | What the film's data scene shows at each of its frames, and the figures its captions state, read from the film itself |
| `FILM_NUMBERS.md` | Every number the film shows, where it is in this dataset, and the checks that compare them (every one matches [checks: check_film.txt]) |

## `prediction/`: the sim-to-real forecast

Published before the real arm is run. `SIM_TO_REAL_PREDICTION.md` states what the real SO-101 will read when it is sent
`commands/sync_write_packets.bin`, the gap we expect between it and this simulation, how that gap was estimated, and how
it will be measured. The estimate is a forecast, not a measurement: the experiment will test the simulation and the
forecast both, and both results will be published here.

- `forecast_joints.csv`: for each joint (`joint` 1 to 6), the error budget in hundredths of an encoder count (`cc`):
  the simulation's own tracking error (`tracking_rms_cc`, `tracking_max_cc`), the servos' published backlash, 0.5 degree
  either way [SOARM100] (`backlash_cc`), the calibration allowance (`calibration_cc`), the encoder's rounding
  (`resolution_rms_cc` [JCGM2008]), and the
  forecast gap over the whole run (`gap_rms_cc`, `gap_max_cc`; `gap_rms_mdeg`, `gap_max_mdeg` in millidegrees).
- `forecast_shapes.csv`: for each shape, its release frame, its clearance through its cutout (um), its largest radius
  (um), and the forecast horizontal displacement of the held shape at its release (`displacement_rms_um`,
  `displacement_max_um`).

## Checks

Every number above was checked against the files, by programs that read the files as you would:

| Check | What it compares | Failing |
|---|---|---|
| C1 | Every `t_ns` is its frame (or tick) x 6,250,000 ns, in `joints.csv`, `pose.csv` and `goals.csv` | 0 |
| C2 | `joints.csv` and `pose.csv` agree on every row's frame, time and refusals; both have 7,745 rows | 0 |
| C3 | Every encoder value in `joints.csv` is its joint's angle at 4,096 counts a turn, rounded half to even, recomputed from the angle column with 2 pi to 19 digits in exact integers (46,470 values) | 0 |
| C4 | `commands/goals.csv`'s goals are `joints.csv`'s, row by row | 0 |
| C5 | At each of the rig's 118 goal keyframes, `joints.csv`'s goals are the rig's goal angles in nanoradians | 0 |
| C6 | Every goal is within its joint's limits as the rig states them | 0 |
| C7 | Every packet in `sync_write_packets.bin`: its header, length, instruction, register, ids, its checksum recomputed, and its six values `goals.csv`'s counts | 0 |
| C8 | Each shape is taken hold of after its grasp time and before its release time, and let go at its release time (frames 511, 1,979, 3,445, 4,885 and 6,354; let go at 1,232, 2,688, 4,144, 5,600 and 7,056) | 0 |
| S1 | Every shape is 40 mm thick (`rig/sorter_measure.txt`) | 0 |
| S2 | The board's underside is 50 mm above the bench | 0 |
| S3 | Each cutout is its shape's outline grown 3 mm a side, within 2 nm (the square, the rectangle, the triangle and the circle; the semicircle's arc is not checked by program) | 0 |
| R | The real-time figures in `realtime/` recomputed from the timing files | as stated above |

The run was made twice: once for the film, and once for this dataset with the joint record added. The two runs' pose
captures (`physics/world_pose_capture.txt` and the film's own) are byte for byte the same, all 7,745 frames [generation]. The
reports are in `checks/`: `check_dataset.txt` (C1 to C8), `check_set.txt` (S1 to S3), `check_film.txt` (the film's
checks, `film/FILM_NUMBERS.md`) and `check_realtime.txt`. How each check works is in `METHODS.md`.

## Licence and how to cite

Copyright 2026 Senua AI. This dataset is licensed under the Creative Commons Attribution 4.0 International licence
(CC BY 4.0, `LICENSE` at the repository's root) [CCBY4]: you may share and adapt it for any purpose, including commercially,
provided you give appropriate credit to Senua AI, link to the licence, and indicate whether changes were made.

Please cite it as:

> Senua AI (2026). Shape sorter: a Senua AI Robotics Core simulation dataset. robotics-core-data,
> datasets/shape-sorter. Licensed under CC BY 4.0.

## Third-party material

The SO-101 arm's geometry, masses and joint layout in `rig/sorter.rig` are taken from TheRobotStudio's SO-ARM100
(https://github.com/TheRobotStudio/SO-ARM100, commit a758567c), licensed under the Apache License 2.0; see `NOTICE` and
`LICENSES/` at the repository's root.
