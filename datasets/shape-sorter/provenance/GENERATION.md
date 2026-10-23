# How the run was generated, and what it used

| Item | Value | Source |
|---|---|---|
| Steps | 7,744 of 6.25 ms (48.4 s of the arm's world); 7,745 frames | `physics/joints.csv` [checks: C1, C2] |
| Computer | Amazon EC2 c7i.2xlarge: 8 vCPUs, 16 GiB, Intel Xeon Scalable (Sapphire Rapids); Linux | [AWSc7i] |
| Processors used | One vCPU | the run was one single-threaded process, pinned to one vCPU |
| Memory, at most | 19,420 kB resident | measured by GNU `time` on the run |
| GPU | None | |
| Models | None: no machine-learning, neural-network or language model of any kind | |
| Paid or remote services | None, beyond the computer itself | |

**The film's run.** The film was drawn from a separate run of the same rig on the same engine, made before this one. Its
POSE capture's SHA-256, recorded before this run began, is
`24369df3aab8a22c623fda04e83a70f3c63e7a62e50b665b0cf61bb0b3cd521a`; `physics/world_pose_capture.txt`, this run's, has
the same SHA-256 (`SHA256SUMS`): the two runs' 7,745 frames are byte for byte the same.

References: `REFERENCES.md`.
