# Methods

## The test, registered first

The reading tests part of a claim made in public on 25 October 2026 [ENACT]: that the Senua AI Robotics Core reads
touch and contact from stock, low-cost servos, with no trained model and no GPU. Its test was written and registered
before any of its data was downloaded or read [P1]; this release holds that test's first check, P1-0, its parts decided in
advance, among them: the same states on two kinds of processor (P1-0a), and each joint's machine with more than one
state and fewer than its distinct changes (P1-0c). The unseen share of a recording read through another arm's
machines was registered to be reported, with no bar.

## The recordings

The LeRobot datasets `lerobot/svla_so101_pickplace` (revision f641879e22172be7e8161d5e6c1503c2d2feb657; 50 episodes,
11,939 frames) and `lerobot/svla_so100_pickplace` (revision 728583b5eaf9e739a7f119e2def466fa1d552402; 50 episodes, 19,631
frames) [LRso101] [LRso100]: a follower arm, driven by a human through a matching leader arm, picking and placing an
object; 30 frames a second. Each frame holds each joint's commanded position (the leader's, `action`) and measured
position (the follower's, `observation.state`). The follower's joints are Feetech STS3215 servos [STS3215] running their
own position loops. The recordings hold no servo load, current or torque.

## The conversion

Each source's one data file (`data/chunk-000/file-000.parquet`, checksums in `checks/check_p1_0.txt`) read with the
parquet reader hyparquet 1.31.5 [HYPQ], each value written as a whole number of hundredths of the source's unit (nearest),
one line a frame. Nothing else is changed: no filtering, no resampling, no bins.

## The reading

For each joint, the engine's one reading of a numeric sensor stream, the same functions its sensor route and the
studio's "Send to Senua" use:
1. **The atom is the change.** The joint's measured position from one frame to the next, signed, at its own resolution.
2. **The machine, once.** The changes of the whole SO-101 recording (its episodes back to back) are read by the
   engine's one inducer, which finds the stream's causal states: two histories are one state when what follows them is
   the same [CY89] [SC01]. Nothing labels the data and nothing is fitted to an outcome.
3. **Each frame, as it arrives.** A new frame's change (from the frame before it in its episode; an episode's first
   frame from itself) lands in one state of the machine, or in none (unseen).

The machines were built from the SO-101 recording and read both recordings; the SO-100 recording, another arm, is held
out of the building.

## Where and how it ran

Each reading ran as one single-threaded process pinned to one core (`taskset -c 1`), on two Amazon EC2 instances
[AWSc7i] [AWSc7g]: `x86b`, a c7i.2xlarge (Intel Xeon, x86-64), and `armb`, a c7g.2xlarge (AWS Graviton3, ARM). The
program was built for each from the same source, and both outputs compared byte for byte.

## What was checked (`checks/check_p1_0.txt`)

The states on both machines are the same bytes for both recordings; every stream frame has its state line; each stream
file is the conversion of its source, by checksum.
