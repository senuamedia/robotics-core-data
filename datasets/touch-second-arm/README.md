# Touch on a second arm, read from its own gripper

A second, different robot arm, the ROBOTIS OpenManipulator-X (five Dynamixel XM430-W350 servos, no force or touch
sensor), from the public Neural Actuation Dataset [NAD], read by the Senua AI Robotics Core: whether its gripper is
holding something, from the gripper's own position and its command alone. No training, no labels, never shown an object:
the one number it uses was identified from 8 recordings of the arm moving with nothing in its gripper.

## How it reads touch

A position servo told to close its gripper on something stops short of where it was told, and stays there: the object
is in the way. With nothing in the gripper it rests close to its command. So the arm is holding something while its
gripper, at rest, sits further from its command than the empty gripper ever rests (`METHODS.md`). The band, how far the
empty gripper ever rests from its command, came from the 8 empty recordings: 8 counts of the servo's 4,096 a turn.

## The result

The test was registered before any of these recordings' data was read. Over the 36 sealed recordings:

| Recordings | Frames | Frames where the arm's touch agrees with NAD's label | Always guessing the more common label |
|---|---|---|---|
| Pick and place, 20 (16 with a weight, 4 with nothing) | 16,253 | **90.2 %** | 65.4 % |
| Lift and hold, 16 (12 with a weight, 4 with nothing) | 13,640 | 99.96 % | 76.7 % |
| Nothing in the gripper, 8 | 6,706 | no frame felt as holding | |

- **The test held**: no false touch in any empty recording, and in pick and place the arm's touch agrees with the label
  nine frames in ten, against six and a half for the best guess made without feeling.
- The same reading, unchanged, as on the SO-101 in [`datasets/weight-lifted`](../weight-lifted/README.md): another maker's
  servos and another gripper.

## Files

| File | What it holds |
|---|---|
| [`readings.tsv`](readings.tsv) | Each sealed recording: its task, payload, frames, frames in touch, frames NAD labels held, and frames agreeing |
| [`identification.tsv`](identification.tsv) | The gripper's band, identified from the 8 empty recordings |
| [`runs/<recording>/`](runs/) | NAD's recording converted to the servo's counts, gzip: `measured.csv.gz` (each frame's measured position, at World's tick), `command.csv.gz` (the command in force at each tick) and `labels.tsv.gz` (NAD's held flag and force label) |
| [`METHODS.md`](METHODS.md) | The data, the identification, the reading and the test |
| [`LICENSE_NAD.txt`](LICENSE_NAD.txt) | NAD's licence (MIT) |
| [`SHA256SUMS`](SHA256SUMS) | Every file's SHA-256 |

## Licence and how to cite

NAD is Copyright (c) 2026 The NeuralActuator Authors, released under the MIT licence (`LICENSE_NAD.txt`); the converted
recordings here keep its terms. Cite NAD as: Dou, Z., et al. (2026), *NeuralActuator: Neural Actuation Modeling for
Robot Dynamics and External Force Perception*, Robotics: Science and Systems. Everything else, the identification and
the readings, is open data under the Creative Commons Attribution 4.0 International licence. Cite it as: Senua AI
(2026), *Touch on a second arm, read from its own gripper*, github.com/senuamedia/robotics-core-data.

[NAD]: https://huggingface.co/datasets/frankzydou/NAD
