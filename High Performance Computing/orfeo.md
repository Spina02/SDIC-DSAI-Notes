
<div align="center">
    <h1>ORFEO cluster commands</h1>
    <h3>Author: Christian Faccio</h3>
    <h5>Email: christianfaccio@outlook.it</h4>
    <h5>Github: <a href="https://github.com/christianfaccio" target="_blank">christianfaccio</a></h5>
</div>

[SLURP Documentation](https://orfeo-doc.areasciencepark.it/HPC/SLURM-basics/)
```bash
Last login: Mon Mar 24 15:58:42 2025 from 149.22.89.76
[christianfaccio@login02 ~]$ hostname
login02.hpc.rd.areasciencepark.it
[christianfaccio@login02 ~]$ ls
[christianfaccio@login02 ~]$ bat job1
[bat error]: 'job1': No such file or directory (os error 2)
[christianfaccio@login02 ~]$ vim job1
[christianfaccio@login02 ~]$ ls
job1
[christianfaccio@login02 ~]$ sbatch job1
Submitted batch job 1061739
[christianfaccio@login02 ~]$ ls
job1  slurm-1061739.out
[christianfaccio@login02 ~]$ bat slurm-1061739.out 
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: slurm-1061739.out
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ epyc001.hpc
   2   │ My first job
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
[christianfaccio@login02 ~]$ cat slurm-1061739.out 
epyc001.hpc
My first job
[christianfaccio@login02 ~]$ sinfo -s
PARTITION AVAIL  TIMELIMIT   NODES(A/I/O/T) NODELIST
EPYC         up 6-06:00:00          5/3/0/8 epyc[001-008]
THIN         up 6-06:00:00        11/0/1/12 fat[001-002],thin[001-010]
GPU          up   18:00:00          1/3/0/4 gpu[001-004]
DGX          up 6-06:00:00          0/2/0/2 dgx[001-002]
H100         up 6-06:00:00          1/1/1/3 dgx[003-005]
GENOA        up 6-06:00:00         6/7/0/13 genoa[001-013]
UPDATE       up    1:00:00          0/0/0/0 
[christianfaccio@login02 ~]$ users
a.elsayed a.suklan afeltrin aspinelli aspinelli avalentini bthastings carlosvelazquez cbaldassi christianfaccio drosa ffabris francescortu ggandolfi ggandolfi gioluc gsantacatterina jackserafini jzacchigna kj_mersimoski lbasile leonardoangellotti lorenzocusin02 lpalaciosflores minvernici msimonutti mtavano nardone nardone nardone ntosato root root s.calabretta s278065 s315505 s316233 stelus vdestasio vpiomponi
[christianfaccio@login02 ~]$ sacctmgr list associations User=christianfaccio
   Cluster    Account       User  Partition     Share   Priority GrpJobs       GrpTRES GrpSubmit     GrpWall   GrpTRESMins MaxJobs       MaxTRES MaxTRESPerNode MaxSubmit     MaxWall   MaxTRESMins                  QOS   Def QOS GrpTRESRunMin 
---------- ---------- ---------- ---------- --------- ---------- ------- ------------- --------- ----------- ------------- ------- ------------- -------------- --------- ----------- ------------- -------------------- --------- ------------- 
     orfeo       dssc christian+       epyc         1         20                node=4                                           5                                     20    02:00:00                             normal                         
     orfeo       dssc christian+      genoa         1         20                node=4                                           5                                     20    02:00:00                             normal                         
     orfeo       dssc christian+        gpu         1         20                node=2                                           5                                     20    02:00:00                             normal                         
     orfeo       dssc christian+       thin         1         20                node=4                                           5                                     20    02:00:00                             normal                         
[christianfaccio@login02 ~]$ squeue -u ntosato
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           1061763      EPYC sample-j  ntosato  R       0:20      1 epyc001
[christianfaccio@login02 ~]$ squeue -u christianfaccio
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
[christianfaccio@login02 ~]$ vim job1
[christianfaccio@login02 ~]$ vim job1
[christianfaccio@login02 ~]$ vim job2
[christianfaccio@login02 ~]$ sbatch job2
Submitted batch job 1061792
[christianfaccio@login02 ~]$ ls
job1  job2  slurm-1061739.out  stderr-sample-job.1061792.err  stdout-sample-job.1061792.out
[christianfaccio@login02 ~]$ bat stdout-sample-job.1061792.out 
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: stdout-sample-job.1061792.out
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ epyc001.hpc
   2   │ Call on all nodes
   3   │ epyc001.hpc
   4   │ epyc002.hpc
   5   │ epyc003.hpc
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
[christianfaccio@login02 ~]$ bat stderr-sample-job.1061792.err 
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: stderr-sample-job.1061792.err   <EMPTY>
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
[christianfaccio@login02 ~]$ srun -N 1 -n 1 -c 1 --time=00:10:00 --mem=10G --partition=THIN --pty BASH
srun: job 1061802 queued and waiting for resources
srun: job 1061802 has been allocated resources
srun: error: thin007: task 0: Exited with exit code 2
slurmstepd: error: execve(): BASH: No such file or directory
[christianfaccio@login02 ~]$ pwd
/u/ipauser/christianfaccio
[christianfaccio@login02 ~]$ ls
job1  job2  slurm-1061739.out  stderr-sample-job.1061792.err  stdout-sample-job.1061792.out  test_orfeo.txt
[christianfaccio@login02 ~]$ touch reverse_test_orfeo.txt
[christianfaccio@login02 ~]$ scp reverse_test_orfeo.txt christianfaccio@149.22.89.76:/Users/christianfaccio/tmp
^C[christianfaccio@login02 ~]$ 
```d