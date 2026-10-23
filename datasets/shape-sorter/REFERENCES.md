# References

Every document in this dataset cites these sources by the key in brackets. Peer-reviewed research is cited for every
physics law the simulation applies; the hardware, protocol, pricing and licence facts are cited to their publishers;
the measured facts of this dataset are cited to the files that hold them.

## Physics and methods (peer-reviewed research, standards and textbooks)

- **[AlartCurnier1991]** Alart, P., and Curnier, A. (1991). A mixed formulation for frictional contact problems prone to
  Newton like solution methods. *Computer Methods in Applied Mechanics and Engineering*, 92(3), 353-375.
- **[ArmstrongHelouvry1994]** Armstrong-Helouvry, B., Dupont, P., and Canudas de Wit, C. (1994). A survey of models,
  analysis tools and compensation methods for the control of machines with friction. *Automatica*, 30(7), 1083-1138.
- **[BIPM2019]** Bureau International des Poids et Mesures (2019). *The International System of Units (SI)*, 9th
  edition. (The standard acceleration of gravity, 9.80665 m/s^2, declared by the 3rd General Conference on Weights and
  Measures, 1901.)
- **[Brogliato2016]** Brogliato, B. (2016). *Nonsmooth Mechanics: Models, Dynamics and Control*, 3rd edition. Springer.
  (Signorini's condition; Newton's law of impact and restitution.)
- **[Featherstone2008]** Featherstone, R. (2008). *Rigid Body Dynamics Algorithms*. Springer.
- **[Goldstein2002]** Goldstein, H., Poole, C., and Safko, J. (2002). *Classical Mechanics*, 3rd edition. Addison
  Wesley. (The Newton-Euler equations of a rigid body; the parallel-axis theorem.)
- **[Gonzalez1996]** Gonzalez, O. (1996). Time integration and discrete Hamiltonian systems. *Journal of Nonlinear
  Science*, 6(5), 449-467.
- **[IEEE754]** IEEE (2019). *IEEE Standard for Floating-Point Arithmetic* (IEEE Std 754-2019). (Rounding to nearest,
  ties to even.)
- **[ItohAbe1988]** Itoh, T., and Abe, K. (1988). Hamiltonian-conserving discrete canonical equations based on
  variational difference quotients. *Journal of Computational Physics*, 76(1), 85-102.
- **[JCGM2008]** Joint Committee for Guides in Metrology (2008). *Evaluation of measurement data: Guide to the
  expression of uncertainty in measurement* (JCGM 100:2008). (Combining independent uncertainties in quadrature; the
  rectangular distribution, standard uncertainty a / sqrt(3) for half-width a.)
- **[McLachlan1999]** McLachlan, R. I., Quispel, G. R. W., and Robidoux, N. (1999). Geometric integration using
  discrete gradients. *Philosophical Transactions of the Royal Society of London A*, 357(1754), 1021-1045.
- **[Moreau1988]** Moreau, J. J. (1988). Unilateral contact and dry friction in finite freedom dynamics. In J. J. Moreau
  and P. D. Panagiotopoulos (Eds.), *Nonsmooth Mechanics and Applications* (CISM Courses and Lectures 302), 1-82.
  Springer.
- **[NakamuraHanafusa1986]** Nakamura, Y., and Hanafusa, H. (1986). Inverse kinematic solutions with singularity
  robustness for robot manipulator control. *Journal of Dynamic Systems, Measurement, and Control*, 108(3), 163-171.
- **[NordinGutman2002]** Nordin, M., and Gutman, P.-O. (2002). Controlling mechanical systems with backlash: a survey.
  *Automatica*, 38(10), 1633-1649.
- **[PfeifferGlocker1996]** Pfeiffer, F., and Glocker, C. (1996). *Multibody Dynamics with Unilateral Contacts*. Wiley.
- **[PopovaPopov2015]** Popova, E., and Popov, V. L. (2015). The research works of Coulomb and Amontons and generalized
  laws of friction. *Friction*, 3(2), 183-190.
- **[Shoemake1985]** Shoemake, K. (1985). Animating rotation with quaternion curves. *Computer Graphics (SIGGRAPH '85)*,
  19(3), 245-254.
- **[Spong2006]** Spong, M. W., Hutchinson, S., and Vidyasagar, M. (2006). *Robot Modeling and Control*. Wiley.
- **[StewartTrinkle1996]** Stewart, D. E., and Trinkle, J. C. (1996). An implicit time-stepping scheme for rigid body
  dynamics with inelastic collisions and Coulomb friction. *International Journal for Numerical Methods in Engineering*,
  39(15), 2673-2691.
- **[Suzuki1990]** Suzuki, M. (1990). Fractal decomposition of exponential operators with applications to many-body
  theories and Monte Carlo simulations. *Physics Letters A*, 146(6), 319-323.
- **[Swope1982]** Swope, W. C., Andersen, H. C., Berens, P. H., and Wilson, K. R. (1982). A computer simulation method
  for the calculation of equilibrium constants for the formation of physical clusters of molecules: application to
  small water clusters. *The Journal of Chemical Physics*, 76(1), 637-649. (The velocity Verlet method.)
- **[Verlet1967]** Verlet, L. (1967). Computer "experiments" on classical fluids. I. Thermodynamical properties of
  Lennard-Jones molecules. *Physical Review*, 159(1), 98-103.
- **[WalkerOrin1982]** Walker, M. W., and Orin, D. E. (1982). Efficient dynamic computer simulation of robotic
  mechanisms. *Journal of Dynamic Systems, Measurement, and Control*, 104(3), 205-211. (The composite rigid body
  method for the mass matrix.)
- **[Wampler1986]** Wampler, C. W. (1986). Manipulator inverse kinematic solutions based on vector formulations and
  damped least-squares methods. *IEEE Transactions on Systems, Man, and Cybernetics*, 16(1), 93-101.
- **[Yoshida1990]** Yoshida, H. (1990). Construction of higher order symplectic integrators. *Physics Letters A*,
  150(5-7), 262-268.

## The arm, its servos and their protocol (publishers' sources)

- **[SOARM100]** TheRobotStudio. *SO-ARM100* (the SO-101 arm), https://github.com/TheRobotStudio/SO-ARM100, commit
  a758567c3978dfeefe282ede0500085a48fe8f78, Apache License 2.0: `Simulation/SO101/so101_new_calib.urdf` (the arm's
  links, masses, inertias and joints); `Simulation/SO101/joints_properties.xml`, class `sts3215` (damping 0.60,
  frictionloss 0.052, armature 0.028, position kp 17.8, forcerange -3.35 to 3.35) and class `backlash` (free play of
  -0.5 to +0.5 degree, "+/- 0.5 deg of backlash"); `Simulation/SO101/README.md` (the STS3215 properties "adapted from
  the Open Duck Mini project").
- **[OpenDuckMini]** Pirrone, A., and contributors. *Open Duck Mini*, https://github.com/apirrone/Open_Duck_Mini (the
  source SO-ARM100 credits for the STS3215's properties).
- **[LeRobot]** Hugging Face. *LeRobot*, https://github.com/huggingface/lerobot, commit
  d3793b7e36522321c1c455f0dc4826db3793dceb, `src/lerobot/robots/so_follower/so_follower.py`, lines 72-77 (the SO-101
  follower's servos: shoulder_pan 1, shoulder_lift 2, elbow_flex 3, wrist_flex 4, wrist_roll 5, gripper 6, each an
  STS3215).
- **[rustypot]** Pollen Robotics. *rustypot*, https://github.com/pollen-robotics/rustypot, commit
  7aa85e13143594ea4fd4ed9f29e3e29d39b5de85, Apache License 2.0, `src/servo/feetech/sts3215.rs` (the STS3215: 4,096
  steps a turn; factory baud rate 1,000,000; torque enable register 40; goal position register 42 and present position
  register 56, each two bytes, sign and magnitude in bit 15).
- **[DXL1]** ROBOTIS. *DYNAMIXEL Protocol 1.0*, https://emanual.robotis.com/docs/en/dxl/protocol1/ (the packet framing
  the Feetech STS servos share: header, id, length, instruction, parameters and checksum; the broadcast id 0xFE; the
  sync write instruction 0x83).
- **[SO101Kit]** Pakronics. *SO-ARM101 low-cost AI arm servo motor kit Pro for LeRobot, assembled version* (product
  SS100046482), https://www.pakronics.com.au/products/so-arm101-low-cost-ai-arm-servo-motor-kit-pro-for-lerobot-assembled-version-ss100046482
  (the arm bought for the physical experiment).

## Computers and operating systems (publishers' sources)

- **[AWSc7i]** Amazon Web Services. *Amazon EC2 C7i instances*, https://aws.amazon.com/ec2/instance-types/c7i/.
- **[LinuxSched]** The Linux man-pages project. *sched(7)*, *sched_setscheduler(2)*, *sched_setaffinity(2)*,
  *clock_nanosleep(2)*, *mlockall(2)*, https://man7.org/linux/man-pages/ (real-time scheduling, absolute-deadline
  sleeps, processor affinity and locked memory).
- **[CCBY4]** Creative Commons. *Attribution 4.0 International* (CC BY 4.0),
  https://creativecommons.org/licenses/by/4.0/; its legal code, `LICENSE` at the repository's root.

## This dataset's own evidence (files in this folder)

- **[generation]** `provenance/GENERATION.md`: the run's computer and memory, and the film run's checksum.
- **[checks]** `checks/`: the reports of every check (`check_dataset.txt`, `check_set.txt`, `check_film.txt`,
  `check_realtime.txt`).
- **[realtime]** `realtime/`: the playback timings.
- **[forecast]** `prediction/forecast_joints.csv`, `prediction/forecast_shapes.csv`.
