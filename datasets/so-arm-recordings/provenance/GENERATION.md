# How the processing was run, and what it used

| Item | Value | Source |
|---|---|---|
| Computers | Amazon EC2 C7i instances, Intel Xeon (x86-64), Linux: one c7i.12xlarge (48 vCPUs, 96 GiB) holding the work, and one c7i.8xlarge and two c7i.2xlarge running simulations beside it, every one running the same simulator binary | [AWSc7i] |
| Processors used | The dataset stage on one vCPU a worker; each simulated episode one single-threaded process on its own vCPU | the run's scripts |
| GPU | None | |
| Models | None: no machine-learning, neural-network or language model of any kind | |
| Paid or remote services | None, beyond the computer and the Hugging Face Hub the recordings were downloaded from | |

References: `REFERENCES.md`.
