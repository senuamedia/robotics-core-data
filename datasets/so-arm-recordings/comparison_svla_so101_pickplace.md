# The comparison set: lerobot/svla_so101_pickplace

The one public recording on which a published study measured the same gap: Antares [Antares] ran MuJoCo [MuJoCo] with MuJoCo Menagerie's SO-101 model [Menagerie] against it, stock and then fitted to the recording's taught episodes, scored on held-out episodes. It is a public repository, not peer-reviewed, and its held-out episodes differ from ours, so the comparison is indicative. Our held-out half of this recording was used in developing the calibration (amendment 13), so World's figures here are development figures.

Root mean square gap, degrees, held-out; lower is better.

| Joint | MuJoCo, stock (Antares) | World, out of the box | MuJoCo, fitted: six parameters shared by every joint (Antares) | World, calibrated |
|---|---|---|---|---|
| shoulder pan | 8.049 | 2.255 | 0.522 | 0.535 |
| shoulder lift | 6.832 | 2.794 | 0.813 | 0.914 |
| elbow flex | 5.355 | 2.315 | 1.121 | 0.911 |
| wrist flex | 3.797 | 1.542 | 0.548 | 0.584 |
| wrist roll | 5.085 | 1.563 | 0.463 | 0.376 |
| five body joints | 6.007 | 2.148 | 0.736 | 0.698 |

MuJoCo's figures are Antares' own (its README's tables, read 25 October 2026). World's are this collection's programs on this recording: out of the box, the SO-101's published model at LeRobot's servo gain; calibrated, each servo's command latency identified from the recording's first half.

- [Antares] Antares: sim-to-real system identification for the SO-100 and SO-101, https://github.com/yih0nk/antares (17 and 18 October 2026).
- [MuJoCo] Todorov, E., Erez, T., and Tassa, Y. (2012). MuJoCo: A physics engine for model-based control. IROS 2012, 5026-5033. doi:10.1109/IROS.2012.6386109.
- [Menagerie] Google DeepMind. MuJoCo Menagerie, robotstudio_so101 (Apache-2.0), https://github.com/google-deepmind/mujoco_menagerie.
