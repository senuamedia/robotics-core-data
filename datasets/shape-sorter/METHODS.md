# Methods: how this dataset was generated and checked

Sources are cited by the keys in `REFERENCES.md`.

## The physics applied

The run was computed by World, the physics engine of the Senua AI Robotics Core. World applies the established laws of
rigid-body mechanics below, each cited to the peer-reviewed research it follows. By the engine's design (the engine itself
is not published), every quantity is held in exact
integer arithmetic and rounded once to the units the data states, to nearest with ties to even [IEEE754]. No
machine-learning, neural-network or language model is used anywhere in generating the data [generation].

**The arm: rigid links on revolute joints.** Each of the arm's six links is a rigid body with the mass, centre of mass
and inertia tensor of the SO-101's published model [SOARM100]. The arm is described in joint coordinates; its mass
matrix is formed from the links' inertias and the joints' kinematics by the composite rigid body method [WalkerOrin1982;
Featherstone2008], under the standard acceleration of gravity, 9.80665 m/s^2 [BIPM2019].

**Time integration that conserves energy.** The arm's motion is advanced by a discrete-gradient method, which conserves
the energy of the unforced system exactly up to rounding [ItohAbe1988; Gonzalez1996; McLachlan1999], composed to fourth
order by symmetric composition [Suzuki1990; Yoshida1990]. The step is 6.25 ms.

**Joint limits.** Each joint's limits are one-sided constraints: a limit pushes and never pulls [Moreau1988;
PfeifferGlocker1996]. A joint arriving at a limit obeys Newton's law of impact with the rig's coefficient of restitution,
0 here [Brogliato2016].

**Joint friction.** Each joint has Coulomb friction [PopovaPopov2015] with a stick state: a joint at rest stays at rest
while the torque on it is within its static friction, and slips against a constant dynamic friction otherwise, stick and
slip decided as a complementarity condition at the velocity level [Moreau1988; StewartTrinkle1996; PfeifferGlocker1996].
Its value, 0.052 N m static and dynamic, is the SO-101's published friction loss [SOARM100].

**The servos.** Each joint is driven by a model of the STS3215 geared servo [Spong2006]: its rotor's inertia reflected
through the gear train (the armature, 0.028 kg m^2), viscous damping (0.60 N m s/rad), and its position loop, a torque
proportional to the goal less the angle (17.8 N m/rad) saturated at its torque limit (3.35 N m). The values are the
SO-101's published ones [SOARM100], adapted there from the Open Duck Mini project [OpenDuckMini]. Friction and the losses
of geared drives follow [ArmstrongHelouvry1994]. The gears' backlash, published as free play of 0.5 degree either way
[SOARM100], is not modelled [NordinGutman2002 surveys its effects]; it is one of the forecast's terms (`prediction/`).

**The encoders.** Each joint's encoder reads 4,096 steps to a turn, the STS3215's resolution [rustypot]: its reading is
the joint's angle in counts, rounded to nearest with ties to even [IEEE754].

**The shapes: free rigid bodies.** Each shape is a rigid prism with its own mass and inertia tensor, moving under gravity
by the Newton-Euler equations [Goldstein2002]; its translation is advanced by velocity Verlet [Verlet1967; Swope1982]
and its rotation, held as a unit quaternion [Shoemake1985], by symmetric composition [Suzuki1990].

**Contact with the bench.** A shape's contacts with the bench are unilateral (Signorini's condition: the bench pushes,
never pulls) and its impacts obey Newton's law with the rig's restitution, 0 [Brogliato2016]; friction is Coulomb's,
its cone with a stick state, coefficient 0.4 [Moreau1988; AlartCurnier1991]; the motion is integrated from event to
event, each impact resolved at its instant [PfeifferGlocker1996; Brogliato2016].

**The grasp.** When the gripper's moving jaw closes on a shape, the shape is held by the jaws' friction (coefficient
0.4) and moves with the gripper as one rigid body, its mass and inertia combined with the gripper's by the parallel-axis
theorem [Goldstein2002]. It stays held while the force the jaws must supply stays within Coulomb's limit for the jaws'
grip [PopovaPopov2015] (no slip occurred: `physics/joints.csv`, `slips`, 0 throughout [checks: C8]), and it is let go
at the rig's release time, leaving with the gripper's velocity at its centre.

**The board.** The board is not a body in the simulation. The clearance between each shape and its cutout's walls is
measured instead, from the frames, at every frame from the release while the shape is within the board's thickness
(`physics/clearance.csv`). Every shape clears its walls by 1.685 mm or more, so no contact with the board would have
occurred.

**The arm's commands.** The goal timeline in the rig was computed before the run by inverse kinematics on the published
SO-101 model, by damped least squares [Wampler1986; NakamuraHanafusa1986]: for each shape an approach, a grasp, a lift,
a carry, a turn to the cutout's angle, a lowering and a release. World plays the timeline as given, each servo's goal
moving linearly between the rig's goal lines.

## How the data was generated

One run of the rig, 7,744 steps of 6.25 ms (48.4 s), on one vCPU of an Amazon EC2 c7i.2xlarge [AWSc7i], using at most
19,420 kB of memory, with no GPU and no model of any kind [generation]. Every file in `physics/` is that run's output; every
file in `commands/` is computed from `rig/sorter.rig` alone; `realtime/` is the command stream played on its schedule.
The film's run and this run produced the same frames, byte for byte [generation].

## How the data was checked

Every number this dataset states was checked against the files by programs that read them as a reader of this dataset
would; their reports are in `checks/` and each check is listed in the README [checks]. Among them, independently of the
engine: every encoder value recomputed from the angle columns with 2 pi to 19 digits in exact integers (46,470 values);
every servo-bus packet's checksum recomputed by the protocol's rule [DXL1]; every cutout's growth over its shape
recomputed by exact integer square roots; and every number the film shows compared with the files
(`film/FILM_NUMBERS.md`).

## The forecast

The sim-to-real forecast in `prediction/` combines independent sources of error in quadrature, and takes the encoder's
rounding as a rectangular distribution of half-width half a count, standard uncertainty 0.29 count [JCGM2008].
