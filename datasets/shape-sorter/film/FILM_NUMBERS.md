# Every number in the film, and where it is in this dataset

The film "Senua AI Robotics Core: the shape sorter" (2 minutes 18 seconds, 1920 x 1080, 30 frames a second) shows the
run in this dataset. This table lists every number the film shows, the file and column it comes from here, and the
check that compares the two. A number the dataset does not hold is marked as such. Sources are cited by the keys in
`REFERENCES.md`; the checks' reports are in `checks/` [checks].

| Scene | Time | The film shows | In this dataset | Check |
|---|---|---|---|---|
| C | 0:17 | Five wooden shapes, **40 mm** thick | `rig/sorter.rig`: each `prism`'s points at z = -0.02 and +0.02 m; `rig/sorter_measure.txt`: each `shape` line's height, 40,000,000 nm | S1 |
| C | 0:17 | A board raised **50 mm** | `rig/sorter_measure.txt`, the `board` line: its underside 50,000,000 nm above the bench | S2 |
| C | 0:17 | Each cutout **3 mm** larger than its shape, a side | `rig/sorter_measure.txt`: each shape's outline (`o`) and cutout (`c`) points | S3 |
| G | 0:34 to 0:47 | Each servo's goal, now (six angles in radians, six decimals), at every film frame | `commands/goals.csv` (`goal_nrad_1` to `_6`) at the step the frame shows | F1, F2 |
| G | 0:34 to 0:47 | World's last eight steps: the step and the gripper's x, y and z (nm), at every film frame | `physics/pose.csv`, `link5_centre_x_nm`, `_y_nm`, `_z_nm` | F1, F3 |
| G | 0:34 | World's frame every **6.25 ms** | `rig/sorter.rig`: `step 0.00625`; `t_ns` in every file | C1 |
| G | 0:34 | Every joint's encoder at **4,096 counts** a turn | `rig/sorter.rig`: `encoder 4096` on each joint; `physics/joints.csv`, `encoder_1` to `_6` | C3 |
| E | 0:46 | Clearance to the wall: circle **2.305 mm**, square **2.391**, rectangle **1.685**, triangle **2.411**, semicircle **1.759** | `physics/clearance.csv`, `least_um` | K1 |
| E | 0:46 | **5 of 5** through; none touched a wall of its cutout | `physics/clearance.csv`: every `least_um` above 0 | K1 |
| F | 1:30 | The rectangle **1.685 mm**, the triangle **2.411 mm** from the nearest wall; slowed four times | `physics/clearance.csv`; `film/footage_clips.txt` (the slow clips' speed, 250 thousandths) | K1 |
| J | 1:38 | **5 of 5** shapes through their cutouts; the closest pass **1.685 mm** | `physics/clearance.csv`: every `least_um` above 0, the smallest 1,685 | K1 |
| J | 1:38 | **5 grasps, 0 slips** | `physics/joints.csv`, the last row: `grasps` 5, `slips` 0, `releases` 5 | K2 |
| J | 1:38 | **7,744 steps**, **48.4 seconds**, **6.25 ms** a step | `physics/joints.csv`: frames 0 to 7,744, the last `t_ns` 48,400,000,000 | C1, K2 |
| J | 1:38 | **19 MB** of memory for the whole run | `provenance/generation.txt`: at most 19,420 kB for the run | |
| K | 1:49 | 0 parts per trillion; 13.6 billion; 1.2 x 10^-26 J; 0.2 J; 24 of 24 | Not in this dataset: internal Senua AI validation results | |

`film_figures.csv` holds the figures as the film's captions state them, read from the film, each as a whole number
under a key:

| Key | The figure | Unit |
|---|---|---|
| 1 to 5 | The clearance shown for the circle, square, rectangle, triangle and semicircle | um |
| 6 | The closest pass | um |
| 7, 8 | Grasps, slips | count |
| 9 | Steps | count |
| 10 | The run's length | ms |
| 11 | The step | us |
| 12 | The encoder's counts a turn | count |
| 13 | Shapes through their cutouts | count |
| 14 | The shapes' thickness | um |
| 15 | The board's raise | um |
| 16 | How much larger each cutout is, a side | um |

`film_data_scene.csv` holds what the data scene (scene G) shows at each of its 391 film frames (film frames 1,017 to
1,407): `film_frame`; `clip_frame` (the frame of the whole-run clip shown, clip 2); `step` (the step the numbers are
for); `goals_opacity_milli` and `log_opacity_milli` (how visible the goals and the step log are, 0 to 1,000); the six
goals as shown (`goal_urad_1` to `_6`, microradians); and the step log's eight rows as shown (`logN_step`, `logN_x_nm`,
`logN_y_nm`, `logN_z_nm`).

The checks:

| Check | What it compares | Failing |
|---|---|---|
| S1, S2, S3 | The set's dimensions (40 mm, 50 mm, 3 mm), against `rig/sorter_measure.txt` (see the dataset's README) | 0 |
| C1, C3 | The step (6.25 ms) in every `t_ns`, and every encoder value at 4,096 counts a turn: the dataset's own checks (its README) | 0 |
| F1 | At every one of the data scene's 391 film frames, the step its numbers are for is the World frame its footage shows (`footage_frame_map.csv`, clip 2) | 0 |
| F2 | At every one of those frames, each of the six goals shown is `commands/goals.csv`'s goal at that step, to the six decimals shown (within half a microradian) | 0 |
| F3 | At every one of those frames, the step log's eight rows are the eight steps up to it, and each gripper x, y and z is `physics/pose.csv`'s link 5 centre at that step, exactly | 0 |
| K1 | The five clearances (2,305, 2,391, 1,685, 2,411 and 1,759 um), the closest pass (1,685 um) and the shapes through (5), against `physics/clearance.csv` | 0 |
| K2 | 5 grasps, 0 slips, 7,744 steps, 48,400 ms and 6,250 us a step, against `physics/joints.csv` | 0 |
| K3 | 4,096 counts a turn (each of the six joints), each shape 40,000 um thick, the board raised 50,000 um, the cutouts 3,000 um larger a side, against `rig/sorter.rig` and `rig/sorter_measure.txt` | 0 |

The numbers are on screen in 341 of the data scene's 391 frames (it fades in and out); every check covers all 391. The
report is `checks/check_film.txt`.

