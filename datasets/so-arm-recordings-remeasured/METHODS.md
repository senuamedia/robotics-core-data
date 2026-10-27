# Methods

What changed since post 2's study, how each arm was calibrated, and how the result was checked. Everything not stated
here is as post 2's [`METHODS.md`](../so-arm-recordings/METHODS.md) has it: the recordings and their selection, the
conversion to each servo's own counts, the held-out half, the model, the commands alone, the gap and the baselines.
Sources are cited by the keys in `REFERENCES.md`.

## Registered first

This measurement was registered before any of its runs (the Robotics Core's record P1-RM), with its checks and their
bars; every World prediction's SHA-256 was recorded and committed before any gap was computed. The fresh arms and their
selection were registered before any of their data was read (the Robotics Core's thesis P3).

## The arms

- **Post 2's 154** (its cutoff set): development arms (odd rank, 74), where the calibration terms were developed, and
  sealed arms (even rank, 80), never used to develop them.
- **26 fresh arms**, from the same crawl of the Hugging Face Hub [LeRobot] and by post 2's rules, each from an author
  none of post 2's recordings came from: the recordings whose names name a compressible object (13) or a rigid one (13),
  one an author, largest first. None of them was used to develop or identify any term. `NOTICE.md` credits each.
- **The comparison arm**, `lerobot/svla_so101_pickplace`, post 2's comparison set.

## The calibration, from the first half of each arm's own recording

World's run of the arm's first half (its calibration window) with the published model, out of the box, is set against
the measured follower:

1. **The command latency**: the whole number of World steps (0 to 30) for which World's run, shifted later by it, is
   nearest the measured follower (mean squared difference over the five body joints), as post 2's.
2. **Each body joint's offset b and sag c**: least squares of the difference d (measured less World, counts) on World's
   torque at the joint (N m), the run shifted by the latency: d = b + c x torque. A real servo yields under load more
   or less than the model's, and an arm's own calibration differs from the model's zero; two numbers a joint, from the
   arm's own recording [Ljung1999].
3. **The jaw's stop**: the gripper's closing limit, the least-squares q for which the jaw follows its command a frame
   late but cannot close past q, over the window's frames (every frame counts, a close on air as much as one on an
   object).

The latency goes into World's servos (each servo reads its goal that much later), the offset and sag into World's
encoders (each reports its angle plus b + c x the torque it carries), the jaw's stop into World's gripper joint as its
lower limit. World then predicts the held-out half from the commands alone, as in post 2. Each arm's values:
`calibration.tsv`.

## Checks, registered before the runs

- **The reproduction (RM-a).** On post 2's arms, World with its own calibration gives the figures the same terms gave
  as a correction after World (0.813 every arm, 0.824 development, 0.798 sealed) to within 0.01 degree: **holds** (0.812,
  0.824, 0.797).
- **Post 2's figures (RM-b).** Out of the box and with the latency alone, today's World gives post 2's medians to within
  0.01 degree: **holds** (1.634 against 1.634; 1.176 against 1.176).
- **The fresh arms (RM-c).** On the 26 fresh arms, World with its own calibration is closer than with the latency alone:
  **holds** (0.869 against 1.244).

Every World run completed with no refused step. The first play of the calibrated twins identified some arms from
calibration runs that had not finished writing; it was found by the reproduction check, discarded before any result was
taken from it, and every calibrated twin was identified and played again.
