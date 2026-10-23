# How the reading was made, and what it used

| Item | Value | Source |
|---|---|---|
| Recordings read | 31,570 frames: 11,939 of the SO-101, 19,631 of the SO-100, six joints each | `streams/`, [checks: check_p1_0.txt] |
| Computers | Amazon EC2 c7g.2xlarge (`armb`: AWS Graviton3, ARM, Linux) and c7i.2xlarge (`x86b`: Intel Xeon, x86-64, Linux) | [AWSc7g], [AWSc7i] |
| Processors used | One core on each | each reading one single-threaded process, pinned to one core (`taskset -c 1`) |
| GPU | None | |
| Models | None: no machine-learning, neural-network or language model of any kind | |
| Paid or remote services | None, beyond the computers themselves | |

References: `REFERENCES.md`.
