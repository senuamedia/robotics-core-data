# Methods

How every recording in this collection was chosen, converted, read by the engine and simulated, and how every number was
checked. Sources are cited by the keys in `REFERENCES.md`; the recordings themselves in `dataset_references.md`.

## The test, registered first

Every rule below was written down and registered before the data it touches was read, as amendments 2 to 16 of the
Robotics Core's test P1 [P1]: the selection, the conversion, the measures, the baselines, the checks that decide, the
split of the arms, the servo gain, the calibration and the zero check. **Development and sealed arms:** before any simulated result was computed, the recordings
of odd rank were set aside as development arms (where the simulator's model is developed, and the real hardware's terms
it lacks are identified) and those of even rank sealed: the simulator is set against a sealed arm only once, with its
final model, and those are the validation results.

## Which recordings

- **The listing.** Every dataset on the Hugging Face Hub tagged `LeRobot` [LeRobot]: 82,394 on 25 October 2026. Each
  is screened by its own description of itself (`meta/info.json`, metadata only, never its data), every one named or
  tagged for the SO arms first: 25,704 descriptions were read, of which 8,051 recordings from 2,122 authors met every
  rule below, far more than were needed.
- **Kept**, by that description alone: a robot of the SO-100 / SO-101 family, named by one of LeRobot's classes for it
  (`so100`, `so101`, `so100_follower`, `so101_follower`, `so_follower`); six measured joint positions and six commanded
  ones, named as the SO arm's six joints; LeRobot's format v2.0 to v3.0; at least 3,000 frames (100 seconds at 30
  frames a second); a licence that allows sharing derived data with credit and without share-alike terms (Apache-2.0,
  MIT, CC BY 4.0, CC0, BSD); not gated, not private. Names of other hardware (another servo, a simulated arm, a second
  arm, other teleoperation devices) are not the family.
- **One dataset from each author.** Each Hub author's largest such dataset, by frames, so every recording is a
  different arm; the authors ranked by it, largest first; the first 200 taken (the first 100 registered, and the set
  widened to 200 before any of the new recordings was read, amendment 15). A dataset excluded in processing (below) is
  replaced by the next author in the ranking: ranks 1 to 298 were processed, and each of the 98 passed over is listed
  with its reason (`excluded.tsv`).

## From the recording to the servo's own counts

The SO arms' joints are Feetech STS3215 servos [STS3215]: each reports its position as a whole number, 4,096 to a turn.
LeRobot stores each position as a decimal on a scale of its own [LeRobotCode]: on its calibrated range (-100 to 100; the
gripper 0 to 100), the count less the range's low end times a step (200 or 100 divided by the range's span in counts),
or in degrees, (the count less the range's middle) times 360 / 4,095 (or 360 / 4,096 in its earlier form). Either way,
each joint's recorded values lie on a lattice exactly one count apart.

So each joint's counts are recovered exactly, from the data itself:

1. **The values.** Each recording's data files (no video) read at its pinned revision, each value written as a whole
   number of millionths of its unit (nearest).
2. **The lattice.** For each joint, measured and commanded separately (the leader arm has its own calibration): its
   distinct values sorted; the smallest gap between neighbours; the least divisor d of 1 to 8 for which every value
   lies on a lattice of that gap divided by d, within the 32-bit floating-point format's own error at the joint's largest
   value (twice its unit in the last place [IEEE754]) plus one millionth; the step then refined over the whole range
   observed. The span the step implies must be between 1 and 4,096 counts (a joint does not turn more than once).
3. **The counts.** Each value divided by its step: a whole number of counts (or half a count from the range's
   middle, when the span is odd). A joint with no lattice, or with a span no servo has, excludes its recording. A
   recording that was not made by servo encoders (a simulation, a resampling, a mixture of differently calibrated arms)
   has no single lattice and is excluded by this rule alone.
4. **The commands.** Each commanded value turned into the goal the follower was actually sent, by LeRobot's own
   conversion [LeRobotCode]: the whole part of the value divided by the follower's step, from its range's low end.

Published as `streams/`: each joint in whole counts from its lowest measured value. A value is the lattice's lowest
plus its count times its step (`lattice.tsv`), so the original decimal is recovered exactly.

## The engine's reading

Each joint's measured stream through the engine's one reading of a numeric sensor stream, the same reading its sensor
route and the studio use:

- **The atom is the change**: the joint's position from one frame to the next, in counts, exact.
- **The machine, once**: the changes of the first half of the recording's episodes (by episode index; at least one),
  back to back, read by the engine's one inducer, which finds the stream's causal states: two histories are one state
  when what follows them is the same [CY89; SC01]. Nothing labels the data and nothing is fitted to an outcome.
- **Every frame, as it arrives**: each frame's change lands in one state of the machine, or in none (unseen). Both halves
  read: the first (taught), and
  the second (held out), whose unseen share says how well a machine built from one half reads the other.

Published as `states/` (every frame's six states) and `reading/` (each joint's machine: its states and distinct
changes; the unseen frames).

## The simulator against the real arm (P1-SR)

For every recording whose robot type is a LeRobot follower of its current calibration, each joint's zero at the middle
of its range [TRS], each joint's positive sense the model's [LeIsaac], and the gripper's 0 its closed end at the model's
lower limit [TRS; LeIsaac].

- **On the model's zero (amendment 16).** An arm is in the simulator test only if, for each body joint, the middle of
  its measured range over the first half of its own recording lies inside the model's range for that joint: the
  model's zero is the middle of each joint's range, and an arm recorded on another zero would start pinned at its
  limits. An arm that fails is the engine's reading only, and says so in its README.
- **The model, out of the box.** The SO-101's published model [TRS] as the shape sorter's rig holds it
  (`datasets/shape-sorter`): six rigid links with their published masses and inertias, Coulomb joint friction with a
  stick state, and each STS3215 as its published damping, rotor inertia and torque limit; the gripper's torque limit
  half, as LeRobot sets every follower's gripper [LeRobotCode]. Each servo's stiffness is the one LeRobot's own setting
  gives it (amendment 11): the published model's servo stiffness, 17.8 N m/rad, was identified at the servo's position
  gain of 32 [BAM], and LeRobot writes a gain of 16 to every follower's servos [LeRobotCode], so 8.9 N m/rad. The
  physics: `datasets/shape-sorter/METHODS.md`. No ground and no objects: the arm alone.
- **The model, calibrated to the arm (amendment 14).** Every real arm answers its commands later than a model servo
  does (the leader's reading, the bus, the follower's own loop). That latency is identified from the first half of the
  arm's own recording, never the held-out half: World's out-of-the-box run of that half (`calibration window`), shifted
  later by a whole number of steps, 0 to 30, is set against the measured follower, and the shift with the least mean
  squared difference is the latency [Ljung1999]. It is identified once for the arm: a development A/B registered before
  it ran (ten development arms) found one latency for the arm as good as one for each servo (median five-joint gaps
  1.266 and 1.272 degrees), so the simpler is used. The calibrated twin then receives every goal that much later. Each
  arm's latency is in `calibration.tsv` and its own `calibration.txt`. Nothing else is fitted.
- **The input: the commands alone.** The first pose is the episode's first measured frame, at rest. Then each frame's
  goal, as the follower was sent it, held from that frame's time to one nanosecond before the next. The simulator never
  sees the measured position after the first frame.
- **The step.** The frame period divided by the least whole number that makes a step of 6.25 ms or less (30 frames a
  second: 5.556 ms, six steps a frame), so every frame falls on a step.
- **The sample.** From each recording's held-out half, episodes in index order, each from its own first frame, until 20
  seconds of recording (set before any data).
- **The prediction.** The simulated encoder (counts from the model's zero) at each frame, out of the box and calibrated.
  Each twin's prediction files were hashed and their SHA-256s committed to the Robotics Core's own record before its
  gap was computed; the same files and SHA-256s are published here (`sim/out_of_the_box/`, `sim/calibrated/`, each
  with its `PREDICTIONS.sha256`).
- **The gap.** The Robotics Core's gap program (the one its own arm's test uses): for each joint, measured less predicted
  at every frame, its root mean square and its largest, also set against the shape sorter's published forecast.
- **Two baselines, through the same program, from the same commands.** B0: the follower is exactly where it was last
  told (frame f - 1's goal at frame f). B1: a frame later still (frame f - 2's goal).
- **The checks (registered).** SR-1, for each of joints 1 to 5: the median over the recordings of World's gap divided by
  B0's is below 1, so the simulator predicts the real follower better than its own command does, on more than half the
  arms. SR-2: the same against B1. Both are reported for World out of the box and calibrated, over every arm in the test,
  and over the development and sealed arms apart (`collection.txt`). The gripper is reported with no bar: the simulated
  jaw holds nothing, so where a real one stops on what it grips shows as gap.
- **The comparison set.** `lerobot/svla_so101_pickplace` is the one public recording on which a published study measured
  the same gap, with MuJoCo [Antares; MuJoCo; Menagerie]. It is simulated the same way and reported apart, beside that
  study's figures (`comparison_svla_so101_pickplace.md`). Its held-out half was used while the calibration was
  developed, so its figures are development figures.

## What the gap is, and what it is for

Out of the box, the gap is each real arm's difference from the pure physics of its published model. The calibration
removes the part of it that is each arm's command latency. What remains is each real arm's own difference: friction
beyond the published value (dust, wear, cable drag), the servos' real position loop, gear backlash, gravity sag through
the gears, each arm's own calibration, the real hard stops. Those are the next terms, identified from these recordings
with established models [Ljung1999; ArmstrongHelouvry1994], always developed on some arms and scored on others never
used. The recordings hold positions only (no current or torque), so a heavier load cannot be told from more friction
here; that is for an arm recorded with known loads.

## Where and how it ran

On Amazon EC2 C7i instances (Intel Xeon, x86-64, Linux) [AWSc7i]: one c7i.12xlarge (48 vCPUs) holding the work and
three more (one c7i.8xlarge, two c7i.2xlarge) running simulations beside it. The dataset stage on one vCPU each, every
simulated episode one single-threaded process on its own vCPU, every host running the same simulator binary. No GPU,
and no machine-learning, neural-network or language model of any kind, anywhere.

## What was checked

Every number in this collection is the programs' own output (`reading/`, `gap/`), summarised by a program
(`summary.tsv`, `simulator_out_of_the_box.tsv`, `simulator_calibrated.tsv`, `collection.txt`); `SHA256SUMS` holds a
checksum of every file.
