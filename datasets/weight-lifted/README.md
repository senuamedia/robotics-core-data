# The weight an arm lifts, read from its own joint positions

A stock SO-101 robot arm (six Feetech STS3215 servos, no force sensor) lifting known weights, from the public Neural
Actuation Dataset [NAD], read by the Senua AI Robotics Core: the weight the arm holds, from the positions its own servos
report, through the Robotics Core's simulator, World, and its published model of the arm. No training, no labels, no
force or current reading, and the reading was never shown a weight: everything it uses was identified from 8 recordings
of the arm moving with nothing in its jaws.

## How it reads a weight

A position servo with a high-ratio gear train behaves differently by direction. While a joint lifts against its load, its
motor carries the load, and the joint settles a little below where it was told to go, by an amount that grows with the
weight. At rest, and while lowering, the gears' friction holds much of the load, a different amount at every stop
[ArmstrongHelouvry1994], and the joint's position says little about the weight. So the arm reads the weight the way a
hand does: while it lifts.

For every frame where a joint lifts (it moves the way its servo pushes, in the recording and in World), the reading
compares how far the real joint sits from World's prediction of the arm with nothing in its jaws, against how far World
says a 500 g weight in the jaws would move it [AAH1988]. Over the lifting frames since the jaw closed on something, the best fit
of the one against the other is the weight [Ljung1999] (`METHODS.md`).

## The result

The test was registered before any of these recordings' data was read; World's predictions were hashed
(`PREDICTIONS.sha256`) before any reading was made. The arm's reading at the end of each of the 16 sealed lift-and-hold
recordings with a payload (grams):

| Payload | Reading, four recordings |
|---|---|
| 200 g | 195, 191, 182, 208 |
| 300 g | 318, 262, 304, 286 |
| 400 g | 357, 378, 372, 344 |
| 500 g | 507, 480, 552, 558 |

- **Mean error 25 g**, against 100 g for the best guess made without feeling the weight (always 300 g, the median of the
  payloads). The test held.
- **The 4 sealed recordings with nothing in the jaws** read nothing: the jaw never closed on anything.
- The reading needs the arm to lift: before its first lift it reads nothing.

## Files

| File | What it holds |
|---|---|
| [`readings.tsv`](readings.tsv) | Each sealed lift-and-hold recording: its payload, the reading at its end, the frames held, touched and in all, and the lifting frames read |
| [`identification.tsv`](identification.tsv) | What was identified from the 8 empty recordings: the command latency, each joint's offset, the jaw's band |
| [`runs/<trajectory>/`](runs/) | Each recording's World runs, gzip: for the sealed recordings World's prediction from the commands alone with nothing in the jaws and with 500 g (`world_0g.csv.gz`, `world_500g.csv.gz`, and their rigs); for the identification recordings World's run with the published model (`world_out_of_the_box.csv.gz`); and NAD's recording converted to the servo's counts (`measured.csv.gz`, `command.csv.gz`, and NAD's held flag and force label, `labels.tsv.gz`) |
| [`PREDICTIONS.sha256`](PREDICTIONS.sha256) | The SHA-256 of every sealed prediction, unpacked, as recorded before any reading |
| [`METHODS.md`](METHODS.md) | The data, the identification, the reading and the test |
| [`LICENSE_NAD.txt`](LICENSE_NAD.txt) | NAD's licence (MIT) |

World's rows (a row each World step of 5.35 ms) have the columns of `datasets/shape-sorter`'s `joints.csv`
([`datasets/shape-sorter/README.md`](../shape-sorter/README.md)): each joint's angle, rate, goal, servo torque and encoder.

## Licence and how to cite

NAD is Copyright (c) 2026 The NeuralActuator Authors, released under the MIT licence (`LICENSE_NAD.txt`); the converted
recordings here (`measured`, `command`, `labels`) keep its terms. Cite NAD as: Dou, Z., et al. (2026), *NeuralActuator:
Neural Actuation Modeling for Robot Dynamics and External Force Perception*, Robotics: Science and Systems. Everything
else, World's predictions, the identification and the readings, is open data under the Creative Commons Attribution 4.0
International licence. Cite it as: Senua AI (2026), *The weight an arm lifts, read from its own joint positions*,
senuamedia/robotics-core-data, `datasets/weight-lifted`.

## References

- **[NAD]** Dou, Z., Onyemelukwe, J. U., Zhang, H., Zhang, H., Guo, M., Tian, Y., Lipiec, M. P., Jacob, J., Liu, C.,
  Chen, P. Y., Ivanov, Y., and Matusik, W. (2026). NeuralActuator: Neural Actuation Modeling for Robot Dynamics and
  External Force Perception. *Robotics: Science and Systems*. Dataset: https://huggingface.co/datasets/frankzydou/NAD,
  revision 88838bce278e381a830a71af83e27dfbb1dac414 (MIT); code: https://github.com/Frank-ZY-Dou/Dynamics-Modeling.
- **[ArmstrongHelouvry1994]** Armstrong-Helouvry, B., Dupont, P., and Canudas de Wit, C. (1994). A survey of models,
  analysis tools and compensation methods for the control of machines with friction. *Automatica*, 30(7), 1083-1138.
  doi:10.1016/0005-1098(94)90209-7.
- **[Ljung1999]** Ljung, L. (1999). *System Identification: Theory for the User* (2nd ed.). Prentice Hall.
- **[AAH1988]** An, C. H., Atkeson, C. G., and Hollerbach, J. M. (1988). *Model-Based Control of a Robot Manipulator*.
  MIT Press.
- **[TRS]** TheRobotStudio. *SO-ARM100*, https://github.com/TheRobotStudio/SO-ARM100 (Apache-2.0): the SO-101's
  published model.
