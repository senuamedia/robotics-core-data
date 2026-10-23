# SO arm recordings: real robot arms against the Senua AI Robotics Core

Public recordings of real, low-cost SO-100 and SO-101 robot arms (each six stock Feetech STS3215 servos), made by 200
different people and published openly on the Hugging Face Hub, run through the Senua AI Robotics Core:

1. **The engine reads every joint, every frame, into causal states**, with no labels, no training, no
   model of any kind and no GPU.
2. **The simulator predicts each real follower arm from the commands it was sent alone**, using the SO-101's published
   physical model, out of the box and then calibrated to each arm from the first half of its own recording (its command
   latency); the difference from what the real arm did is the sim-to-real gap, measured arm by arm on held-out
   episodes, against two command-only baselines.

Every rule (which recordings, the conversion, the measures, the baselines and the checks that decide) was registered
before the data it touches was read (`METHODS.md`). Sources are cited by the keys in `REFERENCES.md`; every recording
is credited to its author in `dataset_references.md` and `NOTICE.md`.

**The recordings:** [`datasets.md`](datasets.md) lists all 200, each with its author, licence and frames, whether the
simulator test covers it, and a link to its own folder under [`recordings/`](recordings/). Before any simulated result
was computed, the recordings were split into development arms (odd rank: where the simulator's model and its
calibration are developed) and sealed arms (even rank: set against the simulator once, with its final model).

| File | What it holds |
|---|---|
| [`datasets.md`](datasets.md) | Every recording taken, with its author, licence and whether it is in the simulator test |
| [`excluded.tsv`](excluded.tsv) | Every recording passed over, with the registered reason |
| [`summary.tsv`](summary.tsv) | One row a recording: its counts and the engine's reading |
| [`simulator_out_of_the_box.tsv`](simulator_out_of_the_box.tsv) | One row an arm: World's gap with the published model, and the two baselines', joint by joint |
| [`simulator_calibrated.tsv`](simulator_calibrated.tsv) | One row an arm: World's gap calibrated to that arm, and the two baselines', joint by joint |
| [`calibration.tsv`](calibration.tsv) | Each arm's command latency, identified from the first half of its own recording |
| [`collection.txt`](collection.txt) | The collection's totals and the simulator's medians and checks (SR-1, SR-2): every arm, development and sealed |
| [`comparison_svla_so101_pickplace.md`](comparison_svla_so101_pickplace.md) | The one recording a published study measured the same gap on, set beside its figures |
| [`recordings/<author>--<name>/`](recordings/) | Each recording's own README, source and lattice, count streams, every frame's states, the engine's reports; and, for each arm in the simulator test, its calibration, World's predictions with their SHA-256s, the measured and baseline tables and the gaps |
| [`METHODS.md`](METHODS.md) | How the recordings were chosen, converted, read, simulated, calibrated and checked |
| [`provenance/GENERATION.md`](provenance/GENERATION.md) | What the processing used |
| [`SHA256SUMS`](SHA256SUMS) | A checksum of every file in this folder |

## What it can and cannot show

It shows how the engine reads real servo recordings, and how far the published physical model of the arm is from real
arms, joint by joint and arm by arm, out of the box and once calibrated to each arm. It does not show what a causal
state means (a grip, a contact): that is a further test. The recordings hold positions only, so no force, friction or
mass is measured here; the gap that remains after calibration is what the next terms of each real arm (its friction,
backlash, sag under load and its own calibration) are identified from, developed on some arms and scored on others.

**One duplicate.** `SkywalkerLi/so101_phase_split` and `nc8304/so101_color_augmented`, from two authors, appear to hold the
same recording: their simulated and baseline gaps agree to the thousandth of a degree. The exact-duplicate check (a
checksum of the whole count streams) did not flag them, so both are counted among the arms; the effect on the medians
is negligible.

## Licence and how to cite

Each recording is its author's, under the licence its author gave it (`NOTICE.md`); the converted streams keep those
terms. Everything else here, the engine's reading, the simulator's predictions, the gaps and the measurements, is open
data under the Creative Commons Attribution 4.0 International licence [CCBY4]. Cite it as: Senua AI (2026), *SO arm
recordings: real robot arms against the Senua AI Robotics Core*, senuamedia/robotics-core-data,
`datasets/so-arm-recordings`; and credit each recording to its author.
