# Hands-on Leonardo Cluster

### Login and General Info

To access the Leonardo cluster, register at Cineca UserDB area following the instructions in the website. 

For everyday login, use the following command:

```
step ssh login '<email>' --provisioner cineca-hpc # login

ssh <username>@login.leonardo.cineca.it
```

if a warning (`WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!`) appears, just run the following command and login:
```
ssh-keygen -f ~/.ssh/known_hosts -R login.leonardo.cineca.it; for keyal in ssh-rsa ecdsa-sha2-nistp256; do for address in login01-ext.leonardo.cineca.it login02-ext.leonardo.cineca.it login05-ext.leonardo.cineca.it login07-ext.leonardo.cineca.it; do ssh-keyscan -t  ${keyal} ${address} | sed "s/\b${address}/login*.leonardo.cineca.it/g" >> ~/.ssh/known_hosts; done; done
ssh <username>@login.leonardo.cineca.it
```

`saldo -b --dcgp` tells how many core-hours (budget) is left for the **group**.

Details on the DCGP module can be found [here](https://leonardo-supercomputer.cineca.eu/hpc-system/#jump-partition). Essentially, it is composed by **1536 nodes** (172032 CPU cores) equipped with two **Intel Sapphire Rapids CPUs**, each with **56 cores**, and reaches over 9 petaFLOPS of sustained performance. All the nodes are interconnected through an **Infiniband HDR interconnect**  network organized in a **Dragonfly+ topology**.

Three-node BullSequana X2140 blade, each node with:
- 2x 56-core Intel Xeon Platinum 8480+ CPU, 2.0 GHz (SapphireRapids)
- 16x 32 GB DDR5-4800 (512 GB)
- 1x SSD 3.84 TB M.2 NVMe
- 1x single port HDR100 network interface (100 Gbps)

### File Systems and Data Management

[link](https://docs.hpc.cineca.it/hpc/hpc_data_storage.html)

For **Data Transfer**, it is possible to transfer small files from local machine to the cluster, but **data movers** are strongly recommended since there is a 10-minutes CPU time limit on login nodes. 

**Data movers** are dedicated, containerized nodes without interactive access, supporting only a limited set of commands (`scp, rsync, sftp, wget, curl, rclone, aws s3 and s3`). User authentication is done via SSH certificates with 2-Factor Authentication or host-based authentication from within CINECA clusters.

*LEONARDO* 

alias: data.leonardo.cineca.it

hostnames and IPs:

- dmover1.leonardo.cineca.it - 131.175.44.50
- dmover2.leonardo.cineca.it - 131.175.44.51
- dmover3.leonardo.cineca.it - 131.175.44.52
- dmover4.leonardo.cineca.it - 131.175.44.53

The authentication is still based on SSH protocol. There are only 2 possible authentication methods:

- *publickey*: it only accepts valid SSH certificates, obtained via 2-Factor Authentication. No private/public keys generated in other ways are accepted.
- *hostbased*: if you are already logged into a CINECA HPC cluster and try to use a datamover from a login node, the SSH daemon on the datamover recognizes you are already authenticated on a CINECA HPC cluster and that is enough.

##### Transfer Tools

- `scp` - secure copy (remote file copy program)

There are 3 possible ways to use scp via datamovers:
1. You need to upload or download data FROM/TO your local machine TO/FROM a CINECA HPC cluster
```
scp /absolute/path/from/file <username>@data.<cluster_name>.cineca.it:/absolute/path/to/
scp  <username>@data.<cluster_name>.cineca.it:/absolute/path/from/file /absolute/path/to/
```
2. You need to transfer files between 2 CINECA HPC clusters
```
ssh -xt <username>@data.<cluster_name_1>.cineca.it scp /absolute/path/from/file <username>@data.<cluster_name_2>.cineca.it:/absolute/path/to/
ssh -xt <username>@data.<cluster_name_1>.cineca.it scp <username>@data.<cluster_name_2>.cineca.it:/absolute/path/from/file  /absolute/path/to/
```
3. You need to transfer files between 2 CINECA HPC clusters using your local machine as a bridge. We strongly suggest not using this option because it has very low transfer performance, each file you move from one cluster to another will pass through your local machine
```
scp -3 <username>@data.<clusterr_name_1>.cineca.it:/absolute/path/from/file  data.<cluster_name_2>.cineca.it:/absolute/path/from/file
```

> **Note**: to list the files and directories in a datamover, use this command from your local machine `sftp <username>@data.<cluster_name>.cineca.it:/path/to/be/listed/`and once in, just type `pwd`or other known commands to understand the structure of the filesystem. Moreover, when using `scp`for data transfer, remember to use the following absolute path for the datamover: `/leonardo/home/userexternal/<username>/` (actually if you are on a different project it could be different, just enter the login node of Leonardo, type `pwd` and copy it after `/home/leonardo/` to get the correct path).

> What worked for me for the transfer between the two clusters of a folder (from data.leonardo to login.leonardo) was:
>```
>ssh -xt <username>@data.leonardo.cineca.it scp -r /leonardo/home/>userexternal/<username>/path_to_folder <username>@login.leonardo.cineca.>it:./
>```
> you can reiterate the command multiple times without a problem, it will just overwrite the files already present in the destination folder.

### Scheduler and Job Submission

[link](https://docs.hpc.cineca.it/hpc/hpc_scheduler.html#scheduler-and-job-submission)

CINECA HPC clusters are accessed via a dedicated set of login nodes. These nodes are intended for simple tasks such as customizing the user environment by installing applications, transferring data, and performing basic pre- and post-processing of simulation data. Access to the compute nodes is managed by the workload manager. To ensures fair access to resources for all users, production jobs must be submitted using a scheduler.

SLURM is used as job scheduling system on Leonardo. 

There are two main modes of using compute nodes:

- **Batch mode**: This mode is intended for production runs. Users must prepare a shell script with all the operations to be executed once the requested resources are available. The job will then run on the compute nodes. Store all your data, programs, and scripts in the $WORK or $SCRATCH filesystems, as these are best for compute node access. You must have valid active projects to run batch jobs, and be aware of any specific policies regarding project budgets on our systems.

- **Interactive mode**: Jobs submitted in this mode are similar to batch mode in that the user must specify the resources to allocate. The job is then managed like any other submitted job. The key difference from batch mode is that once the job is running, the user can interactively execute applications within the limits of the allocated resources. All allocated resources are available for the entire requested walltime (and consequently billed) during the submission process.

#### Basic Usage of SLURM 

Typically, you create a **batch job**, which is a file (a shell script in UNIX) containing the set of commands you want to run. This file also includes `directives` that specify the job’s characteristics and resource requirements, such as the number of processors and CPU time needed. Once you create your job script, you can reuse it or modify it for subsequent runs.

1. Create a job script with SLURM `directives``
2. Submit the job using `sbatch`command 
3. Monitor the job using commands like `squeue` and `scontrol``
4. Cancel the job if needed using `scancel`command

Example using a wall time of 1h and requesting 1 node with 32 cores:

```
#!/bin/bash

#SBATCH --nodes=1                    # 1 node
#SBATCH --ntasks-per-node=32         # 32 tasks per node
#SBATCH --time=1:00:00               # time limit: 1 hour
#SBATCH --error=myJob.err            # standard error file
#SBATCH --output=myJob.out           # standard output file
#SBATCH --account=<Project Account>  # project account
#SBATCH --partition=<partition_name> # partition name
#SBATCH --qos=<qos_name>             # quality of service

./my_application
```

For the main SLURM commands refer to the previous link for complete details.