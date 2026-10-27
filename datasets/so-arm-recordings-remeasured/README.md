# SO arm recordings, measured again: World calibrated to each arm by World itself

The real arms of [`datasets/so-arm-recordings`](../so-arm-recordings/README.md) (post 2's study: public recordings of
real SO-100 and SO-101 arms, each six stock Feetech STS3215 servos) set against the Senua AI Robotics Core's simulator,
World, again: the same 154 arms and held-out episodes, 26 arms never used before, and the one arm a published study
measured too. World now calibrates itself to each arm, from the first half of that arm's own recording:

1. **the arm's command latency** (as in post 2);
2. **each joint's offset and how far it sags under load**, two numbers a joint, written into World's model of the arm (its
   encoders), so World itself makes the calibrated prediction;
3. **where the arm's jaw stops closing**, written into World's gripper as its closing limit.

Nothing else changes: the SO-101's published physical model, World's exact physics, each arm predicted from the
commands it was sent alone, the gap measured on the held-out half as in post 2 (`METHODS.md`). Every rule was registered
before the runs it decides, and every prediction's SHA-256 was published before any gap was computed.

## The result

Median over the arms of each arm's root mean square gap on its held-out half, degrees; lower is better.

| Arms | Out of the box | Post 2 (latency) | World's own calibration | The command itself (B0) |
|---|---|---|---|---|
| Post 2's arms, five body joints (154) | 1.634 | 1.176 | **0.812** | 2.702 |
| Development arms (74) | 1.618 | 1.124 | **0.824** | 2.721 |
| Sealed arms (80) | 1.635 | 1.213 | **0.797** | 2.655 |
| 26 fresh arms, five body joints | 1.647 | 1.244 | **0.869** | 2.754 |
| Post 2's arms, the gripper | 3.771 | 3.548 | **3.187** | 4.758 |
| 26 fresh arms, the gripper | 3.413 | 3.423 | **3.222** | 4.661 |

- **Arm by arm:** the five joints come closer than with the latency alone on 144 of 154 arms and 24 of 26 fresh arms,
  closer than out of the box on 152 of 154 and 25 of 26. The gripper with its jaw stop comes closer on 106 of 154 arms
  (17 the same, 31 further) and 16 of 26 (2 the same, 8 further).
- **The fresh arms** were never used to develop or identify any term: 26 recordings from 26 authors not among post 2's,
  13 named for compressible objects (sponges, tissues, a napkin, a plush toy) and 13 for rigid ones (cubes, blocks, Lego,
  a marker, a pen).
- **Post 2's figures reproduce** on today's World: out of the box 1.634 (post 2: 1.634), the latency alone 1.176
  (1.18).
- **The comparison arm** (`lerobot/svla_so101_pickplace`): 0.598, against a published study's MuJoCo model fitted to the
  same recording, 0.736 with one latency and 0.656 with one a joint
  ([`comparison_svla_so101_pickplace.md`](comparison_svla_so101_pickplace.md)).

## Files

| File | What it holds |
|---|---|
| [`simulator_world_calibrated.tsv`](simulator_world_calibrated.tsv) | One row an arm (its set: D post 2's, S fresh, C the comparison arm): World's gap with its own calibration and the jaw stop, and the two command baselines', joint by joint, in post 2's columns (thousandths of a degree) |
| [`simulator_out_of_the_box.tsv`](simulator_out_of_the_box.tsv) | The same with the published model alone |
| [`calibration.tsv`](calibration.tsv) | Each arm's identified calibration: command latency (ms), each body joint's offset (counts) and sag (counts per N m of the joint's torque), the jaw's stop (counts and degrees) |
| [`collection.txt`](collection.txt) | The medians per joint and the two baselines' checks, every arm, development, sealed, fresh and the comparison arm |
| [`recordings/<author>--<name>/`](recordings/) | Each arm's World runs: `sim/world_calibrated/` and `sim/out_of_the_box/` (each episode's rig and World's prediction, gzip, and their SHA-256s, of the files unpacked), `gap/` (each joint's gap, World's and the baselines'); for the fresh arms also `sim/measured/` (the measured follower and the baselines, counts) and `source.tsv` (the recording, its revision, licence and author) |
| [`METHODS.md`](METHODS.md) | What changed since post 2, how each arm was identified, and the checks |
| [`NOTICE.md`](NOTICE.md), [`REFERENCES.md`](REFERENCES.md) | The fresh recordings credited; the sources |

The measured tables of post 2's arms and the comparison arm are in post 2's folder,
[`datasets/so-arm-recordings/recordings/`](../so-arm-recordings/recordings/), unchanged: the same held-out episodes.

## Licence and how to cite

Each recording is its author's, under the licence its author gave it (`NOTICE.md`, and post 2's `NOTICE.md`). Everything
else here, the simulator's predictions, the calibrations and the gaps, is open data under the Creative Commons
Attribution 4.0 International licence [CCBY4]. Cite it as: Senua AI (2026), *SO arm recordings, measured again: World
calibrated to each arm by World itself*, senuamedia/robotics-core-data, `datasets/so-arm-recordings-remeasured`; and
credit each recording to its author.
