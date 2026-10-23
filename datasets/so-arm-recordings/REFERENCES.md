# References

Every recording read here is credited in `dataset_references.md`, at the revision read.

## The software and models the recordings came from (publishers' sources)

- **[LeRobot]** Hugging Face. *LeRobot* datasets on the Hugging Face Hub, tag `LeRobot`, https://huggingface.co/datasets?other=LeRobot; listed 25 October 2026.
- **[LeRobotCode]** Hugging Face. *LeRobot*, https://github.com/huggingface/lerobot, commit d3793b7e36522321c1c455f0dc4826db3793dceb (Apache-2.0): `src/lerobot/motors/motors_bus.py` (`_normalize`, `_unnormalize`: the calibrated scales and the conversion of a command to a servo goal) and `src/lerobot/robots/so_follower/so_follower.py` (the gripper's torque limit, 500 of 1,000).
- **[TRS]** TheRobotStudio. *SO-ARM100*, https://github.com/TheRobotStudio/SO-ARM100, commit a758567c3978dfeefe282ede0500085a48fe8f78 (Apache-2.0): `Simulation/SO101/so101_new_calib.urdf`, `joints_properties.xml` and the README (each joint's zero at the middle of its range; the gripper 0 closed, 100 open).
- **[LeIsaac]** LightwheelAI. *LeIsaac*, https://github.com/LightwheelAI/leisaac, commit 24d3bcd3f1e4585740fc79921782c41617237812 (Apache-2.0): `source/leisaac/leisaac/utils/robot_utils.py` and `assets/robots/lerobot.py` (each LeRobot joint mapped onto the model's joint angle with the same sign).
- **[STS3215]** Feetech. *STS3215 serial bus servo*: 4,096 positions to a turn; the SO arms' joint servo [TRS].
- **[BAM]** Rhoban. *BAM: Better Actuator Models*, https://github.com/Rhoban/bam, commit e9a619d5: the STS3215 identified at the factory firmware gain, P = 32 (`params/feetech_sts3215_7_4V`, `feetech/actuator.py`, `to_mujoco.py`); and Duclusaud, M., et al. (2024), extended friction models for the physics simulation of servo actuators, arXiv:2410.08650.
- **[Antares]** Antares: sim-to-real system identification for the SO-100 and SO-101 in MuJoCo, https://github.com/yih0nk/antares (17 and 18 October 2026; a public repository, not peer-reviewed); its README's tables read 25 October 2026.
- **[Menagerie]** Google DeepMind. *MuJoCo Menagerie*, `robotstudio_so101` (Apache-2.0), https://github.com/google-deepmind/mujoco_menagerie.
- **[HYPQ]** hyparquet 1.31.5, a dependency-free Apache Parquet reader, https://github.com/hyparam/hyparquet (MIT).

## Causal states and the physics (peer-reviewed)

- **[CY89]** Crutchfield, J. P., and Young, K. (1989). Inferring statistical complexity. *Physical Review Letters*, 63(2), 105-108. doi:10.1103/PhysRevLett.63.105.
- **[SC01]** Shalizi, C. R., and Crutchfield, J. P. (2001). Computational mechanics: Pattern and prediction, structure and simplicity. *Journal of Statistical Physics*, 104(3-4), 817-879. doi:10.1023/A:1010388907793.
- **[IEEE754]** IEEE (2019). *IEEE Standard for Floating-Point Arithmetic*, IEEE 754-2019. doi:10.1109/IEEESTD.2019.8766229.
- **[ArmstrongHelouvry1994]** Armstrong-Helouvry, B., Dupont, P., and Canudas de Wit, C. (1994). A survey of models, analysis tools and compensation methods for the control of machines with friction. *Automatica*, 30(7), 1083-1138. doi:10.1016/0005-1098(94)90209-7.
- **[MuJoCo]** Todorov, E., Erez, T., and Tassa, Y. (2012). MuJoCo: A physics engine for model-based control. *IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)*, 5026-5033. doi:10.1109/IROS.2012.6386109.
- **[Ljung1999]** Ljung, L. (1999). *System Identification: Theory for the User* (2nd ed.). Prentice Hall.
- The simulator's physics and its sources: `datasets/shape-sorter/METHODS.md` and `datasets/shape-sorter/REFERENCES.md`.

## Computers (publishers' sources)

- **[AWSc7i]** Amazon Web Services. *Amazon EC2 C7i instances*, https://aws.amazon.com/ec2/instance-types/c7i/.
- **[AWSc7g]** Amazon Web Services. *Amazon EC2 C7g instances* (AWS Graviton3), https://aws.amazon.com/ec2/instance-types/c7g/.
- **[CCBY4]** Creative Commons. *Attribution 4.0 International* (CC BY 4.0), https://creativecommons.org/licenses/by/4.0/; its legal code, `LICENSE` at the repository's root.

## The test

- **[P1]** Senua AI (2026). Thesis P1, amendments 2 to 16, each registered 25 October 2026 before the data it touches was read, in the Senua AI Robotics Core's own records (not part of this repository); its rules are set out in `METHODS.md`.
