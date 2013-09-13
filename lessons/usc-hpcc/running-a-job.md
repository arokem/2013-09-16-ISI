---
layout: lesson
root: ../..
github_username: johnmehringer
bootcamp_slug:  2013-09-16-ISI
title: Running a Job 
---
**Material by John Mehringer**

##Job Life Cycle 

Jobs can be run interactively or in batch mode by submitting a PBS script. Once a job is submitted it will be routed into a Torque resource queue and waits until hardware is allocated for it.  After a job completes the hardware is returned to the compute pool and the job is removed from the queue.

![Linux Cluster Racks](./images/hpcc_racks.jpg )

##Queues 

By default jobs submitted to the cluster will be routed into one of the following queues based on the job attributes:

**Main Queue:** The main queue is the default queue for submitted jobs. There is a limit of 24 hours and/or 99 nodes for all jobs submitted to the main queue, and a cap of ten simultaneous running jobs for any single user.  Any additional jobs will be held in the queue until the running job(s) complete. (1hr > walltime <= 24hrs and 4 > nodes <= 99)

**Quick Queue:** All jobs in the main queue with requested wall-clock time of less than an hour and a node count of four or less are transferred automatically to the quick queue. There is a limit of 20 quick-queue jobs per user, and a total of 100 jobs can be in the quick queue at any given time. (walltime <= 1hr and nodes <= 4) 

**Large Queue:** All jobs that require more than 100 nodes are automatically transferred to the large queue. The large queue is limited to one job per user at a time, with a requested wall-clock time limit of 300 hours.

**Largemem Queue:** Jobs which need large amounts of memory up to 1TB on a single node.  Only 1 running job and 1 node can be in use at a time. Runtime for these jobs can be up to 336 hours.   

The current list of resource limits can be found [here](http://hpcc.usc.edu/support/infrastructure/account-resource-limits/).


##Interactive Job Submission Example

```
[jmehring@hpc-login2 ~]$ qsub -I -l walltime=00:05:00 -l nodes=1:ppn=8:myri -l mem=12g
qsub: waiting for job 5046521.hpc-pbs.hpcc.usc.edu to start
qsub: job 5046521.hpc-pbs.hpcc.usc.edu ready

----------------------------------------
Begin PBS Prologue Thu Sep 12 17:26:38 PDT 2013
Job ID:            5046521.hpc-pbs.hpcc.usc.edu
Username:          jmehring
Group:             its-ar
Name:              STDIN
Queue:             quick
Shared Access:     no
All Cores:         no
Nodes:             hpc1118 
TMPDIR:            /tmp/5046521.hpc-pbs.hpcc.usc.edu
End PBS Prologue Thu Sep 12 17:26:39 PDT 2013
----------------------------------------
[jmehring@hpc1118 ~]$ pwd
/home/rcf-40/jmehring
[jmehring@hpc1118 ~]$ source /usr/usc/python/enthought/default/setup.sh
[jmehring@hpc1118 ~]$ which python 
/usr/usc/python/enthought/default/bin/python
[jmehring@hpc1118 ~]$ cd Sources/cluster-tests/python
[jmehring@hpc1118 python]$ python calculate-pi.py 
3.1419744
[jmehring@hpc1118 python]$ exit
logout

qsub: job 5046521.hpc-pbs.hpcc.usc.edu completed
[jmehring@hpc-login2 ~]$ 
```

##PBS Batch Script Job

Example PBS script to be submitted. All PBS parameters should be at the top of the script and have a line starting with \#PBS.  The following requests a 1hr runtime on 2 nodes with 16 processors on each node.  It also requests to run only on the IB cluster nodes and each node needs to have 31GB of RAM available. The -k and -j flags will save all of the stderr and stdout from the job to a single file. This job counts the number of words in a directory of files using hadoop and python.  (The python code was copied from [this](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/) tutorial.)

```
[jmehring@hpc-login2 hadoop]$ cat count-words.pbs 
# !/bin/bash
#PBS -k eo
#PBS -j eo
#PBS -l walltime=01:00:00
#PBS -l nodes=2:ppn=16:IB
#PBS -l mem=31g

echo $(date) - PBS Job Started

source /usr/usc/python/enthought/default/setup.sh
source /usr/usc/hadoop/default/setup.sh

setup-and-start-hadoop-on-hpcc

cd /home/rcf-40/jmehring/Sources/cluster-tests/python/hadoop 
rsync -aq data /scratch

./count-words.sh

rsync -aq /scratch/results/ /home/rcf-proj/hpcc/jmehring/hadoop/results-$(date +%s)

echo $(date) - PBS Job Finished
```

This is an example of how to submit your PBS script using the qsub command. After running the command it will return a job id if the job has been submitted successfully.


```
jmehring@hpc-login2 hadoop]$ qsub count-words.pbs 
5052198.hpc-pbs.hpcc.usc.edu
```

To check on the job status use the qstat commaned followed by your username. In this case, one job has already completed ('C' state) and another job has been queued and is waiting to run.

```
[jmehring@hpc-login2 hadoop]$ qstat -u jmehring

hpc-pbs.hpcc.usc.edu: 
                                                                         Req'd  Req'd   Elap
Job ID               Username    Queue    Jobname          SessID NDS   TSK    Memory Time  S Time
-------------------- ----------- -------- ---------------- ------ ----- ------ ------ ----- - -----
5052115.hpc-pbs.     jmehring    quick    STDIN             29074     2     14    --  01:00 C 00:12
5052198.hpc-pbs.     jmehring    quick    count-words.pbs     --      2     32   31gb 01:00 Q   -- 
```

To get an estimate of when a job might start use the showstart command.  This is an estimate only so your job may start earlier.

```
[jmehring@hpc-login2 hadoop]$ showstart 5052198
job 5052198 requires 32 procs for 1:00:00

Estimated Rsv based start in                00:00:00 on Fri Sep 13 09:00:45
Estimated Rsv based completion in            1:00:00 on Fri Sep 13 10:00:45

Best Partition: torque


NOTE:  job is running
```

---
[Overview](overview.html) -- **Running a Job** -- [Best Practices](best-practices.html)

