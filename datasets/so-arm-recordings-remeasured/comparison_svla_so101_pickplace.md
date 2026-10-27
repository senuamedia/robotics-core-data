# The comparison arm: lerobot/svla_so101_pickplace, measured again

The one public recording on which a published study measured the same gap: Antares [Antares] ran MuJoCo [MuJoCo] with
MuJoCo Menagerie's SO-101 model [Menagerie] against it, stock and then fitted to the recording's taught episodes, scored
on held-out episodes. It is a public repository, not peer-reviewed, and its held-out episodes differ from ours, so the
comparison is indicative. This recording was used in developing post 2's calibration, so World's figures here are
development figures.

Root mean square gap, degrees, held-out; lower is better.

| Joint | MuJoCo, stock (Antares) | World, out of the box | MuJoCo, fitted, six parameters shared (Antares) | World, post 2 (latency) | World, its own calibration |
|---|---|---|---|---|---|
| shoulder pan | 8.049 | 2.255 | 0.522 | 0.535 | **0.380** |
| shoulder lift | 6.832 | 2.795 | 0.813 | 0.914 | **0.725** |
| elbow flex | 5.355 | 2.315 | 1.121 | 0.911 | **0.886** |
| wrist flex | 3.797 | 1.542 | 0.548 | 0.584 | **0.471** |
| wrist roll | 5.085 | 1.563 | 0.463 | 0.376 | **0.335** |
| five body joints | 6.007 | 2.148 | 0.736 | 0.697 | **0.598** |

Antares' MuJoCo model fitted with each joint's own parameters: 0.656 over the five joints (its README). MuJoCo's figures
are Antares' own (its README's tables, read 25 October 2026). World's are this collection's programs on this recording:
out of the box, the SO-101's published model; post 2, each arm's command latency; its own calibration, the latency, each
joint's offset and sag under load, and the jaw's stop, identified from the recording's first half (`METHODS.md`).

- [Antares] Antares: sim-to-real system identification for the SO-100 and SO-101, https://github.com/yih0nk/antares (17 and 18 October 2026).
- [MuJoCo] Todorov, E., Erez, T., and Tassa, Y. (2012). MuJoCo: A physics engine for model-based control. IROS 2012, 5026-5033. doi:10.1109/IROS.2012.6386109.
- [Menagerie] Google DeepMind. MuJoCo Menagerie, robotstudio_so101 (Apache-2.0), https://github.com/google-deepmind/mujoco_menagerie.
