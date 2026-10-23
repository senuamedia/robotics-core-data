# The sim-to-real forecast for the shape sorter

Published on 24 October 2026, before any measurement on a real arm. Sources are cited by the keys in `REFERENCES.md`.

This is an estimate. It states the gap we expect between the simulation and a real SO-101 arm sent the same commands,
and how we arrived at it. The physical experiment will measure the real gap; we will publish the measurement beside
this forecast, together with how good the forecast itself turned out to be, whatever either shows.

## What the simulation predicts

A real SO-101 follower arm [SOARM100; SO101Kit], its six STS3215 servos on one bus with ids 1 to 6 in the joint order of
the data [LeRobot], set up as `rig/sorter.rig` describes, is sent `commands/sync_write_packets.bin`, one packet every
6.25 ms from tick 0, starting from the run's first pose. The simulation predicts:

1. **The servos' positions.** At every tick k, each servo's present position (register 56, counts from the model's zero,
   4,096 to a turn [rustypot]) reads `physics/joints.csv`, frame k, `encoder_1` to `encoder_6`.
2. **The shapes.** Each of the five shapes is taken hold of, carried, turned to its cutout's angle, lowered in and let
   go above its cutout, as the film shows [checks: C8; physics/clearance.csv].

## The gap we expect

**The joints.** Over the whole run, the root mean square difference between each servo's measured position and the
predicted one [forecast: forecast_joints.csv]:

| Joint | Forecast gap, root mean square over the run | Forecast gap, largest |
|---|---|---|
| 1 shoulder_pan | 1.51 degrees (17.20 counts) | 6.4 degrees |
| 2 shoulder_lift | 2.05 degrees (23.29 counts) | 4.8 degrees |
| 3 elbow_flex | 1.39 degrees (15.82 counts) | 4.0 degrees |
| 4 wrist_flex | 0.88 degree (10.02 counts) | 2.7 degrees |
| 5 wrist_roll | 1.50 degrees (17.05 counts) | 5.7 degrees |
| 6 gripper | 5.72 degrees (65.08 counts) | 9.6 degrees |

**The shapes.** At each release, the held shape's position across the board (in x and y) is forecast to differ from the
simulation's by 5.7 to 6.6 mm (root mean square), and by up to 18.5 to 21.0 mm at worst [forecast: forecast_shapes.csv]:

| Shape | Its clearance in the simulation | Forecast displacement, root mean square | Forecast displacement, worst |
|---|---|---|---|
| 1 circle | 2.305 mm | 6.5 mm | 20.1 mm |
| 2 square | 2.391 mm | 6.5 mm | 20.8 mm |
| 3 rectangle | 1.685 mm | 5.8 mm | 19.0 mm |
| 4 triangle | 2.411 mm | 6.6 mm | 21.0 mm |
| 5 semicircle | 1.759 mm | 5.7 mm | 18.5 mm |

Every forecast displacement is larger than its shape's clearance. **So we expect that, replaying these commands as they
are, most shapes will touch the walls of their cutouts on the real arm.** The simulation does not model contact with the
board (`METHODS.md`), so whether a shape that touches a wall still drops through is not forecast. Closing this gap, by
measuring the real servos, their backlash and the calibration and bringing them into the simulation, is the work the
measurement starts.

## How the forecast was made

For each joint, four independent sources of error, combined in quadrature for the root mean square and added for the
largest [JCGM2008]. Each is an assumption, stated so it can be tested:

1. **The servo's own lag.** The simulated servo trails its goal by the simulation's own tracking error, the goal less
   the encoder (`commands/goals.csv` against `physics/joints.csv`). We assume the real servo's lag can differ from the
   model's by up to the model's whole lag: the model's stiffness, damping and torque are the SO-101's published values
   [SOARM100], not yet measured on this arm.
2. **Backlash.** The servos' gears have free play of 0.5 degree either way, as the SO-101's published model gives it
   [SOARM100]; the simulation does not include it [NordinGutman2002].
3. **Calibration.** Each servo's zero is set to the model's zero to within 0.5 degree (our assumption, to be checked
   against the calibration readings).
4. **The encoder's resolution.** Its reading is rounded to the nearest of 4,096 steps a turn [rustypot]: half a count at
   most, a standard uncertainty of 0.29 count as a rectangular distribution [JCGM2008].

For the shapes, the arm is all but still at each release, so each joint's error there is its own lag at that moment (the
goal less the encoder at the frame before the release) with the other three terms. Each joint's error moves the held
shape across the board by its horizontal lever: the joint's axis in the world (from the rig and `physics/pose.csv`)
crossed with the line from the joint to the shape's centre [Goldstein2002], plus the shape's own largest radius for the
part of the axis that is upright (its corners turning). Vertical movement does not bear on the walls and is left out.
The lag of the shoulder and the elbow under gravity, and the calibration and backlash of the shoulder pan (whose lever
is about 27 cm), contribute most.

## How it will be measured

- **The arm and the set.** The SO-101 bought on 22 October 2026, an assembled SO-ARM101 Pro [SO101Kit]. A board and five
  shapes made to the rig's dimensions: shapes 40 mm thick, each cutout 3 mm larger than its shape a side, the board
  raised 50 mm above the bench [checks: S1 to S3].
- **Calibration.** Before the run, each servo's zero is set to the model's zero and its direction checked against the
  model's sense of rotation; the calibration readings are published with the run.
- **The run.** The packets are played from a Linux computer with real-time scheduling [LinuxSched], as `realtime/` was
  [realtime], over the servo bus at the servos' factory rate of 1,000,000 baud [rustypot]. At every tick the six servos'
  present positions are read back and recorded with the tick's time.
- **The joint gap.** For each joint: the root mean square and the largest absolute difference between the recorded
  position and the predicted one, over all 7,745 ticks, in counts and in degrees (one count is 360 / 4,096 = 0.0879
  degree).
- **The shapes.** For each shape: whether it passed through its cutout, whether it touched a wall, and its horizontal
  displacement at the release computed from the recorded joint positions through the same model.
- **Published.** The raw recordings (every tick's six readings and its time), the calibration readings, and the gap.

## How the forecast will be judged

For each joint, the forecast **holds** if the measured root mean square gap is at or below the forecast's, and is
reported with its ratio (measured over forecast: below 1 the forecast was cautious, above 1 it was not). For each shape,
the measured displacement is set against the forecast's, and the outcome (through, touching, or stopped) against the
expectation above. Each of the four sources will also be measured on its own where it can be (the servos' lag, the
backlash, the calibration readings), so that the forecast's errors can be traced to their causes.

## What the simulation does not include

- **The servo model's measurement.** Its parameters are the SO-101's published values [SOARM100], not measured on this
  arm.
- **Backlash** (0.5 degree either way [SOARM100]).
- **The board as a body.** Shapes are not simulated against the board; their clearance is measured (`METHODS.md`).
- **The set as built.** The real board, shapes and bench differ from the model within their making's tolerances, and
  the shapes' friction against the jaws and the bench is not measured yet.
- **Mounting.** The arm's base is modelled as fixed; the real base is clamped to the bench.
