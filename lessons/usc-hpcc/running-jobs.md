---
layout: lesson
root: ../..
github_username: johnmehringer
bootcamp_slug:  2013-09-16-ISI
title: Running Jobs 
---
**Material by John Mehringer**

##Job Life Cycle 

Jobs can be run interactively or in batch mode by submitting a PBS script. Once a job is submitted it will be routed into a Torque resource queue and waits until hardware is allocated for it.  After a job completes the hardware is returned to the compute pool and the job is removed from the queue.

##Queues 

![Linux Cluster Racks](./images/hpcc_racks.jpg )

By default jobs submitted to the cluster will be routed into one of the following queues based on the job attributes:

Main Queue: The main queue is the default queue for submitted jobs. There is a limit of 24 hours and/or 99 nodes for all jobs submitted to the main queue, and a cap of ten simultaneous running jobs for any single user.  Any additional jobs will be held in the queue until the running job(s) complete. (1hr > walltime <= 24hrs and 4 > nodes <= 99)

Quick Queue: All jobs in the main queue with requested wall-clock time of less than an hour and a node count of four or less are transferred automatically to the quick queue. There is a limit of 20 quick-queue jobs per user, and a total of 100 jobs can be in the quick queue at any given time. (walltime <= 1hr and nodes <= 4) 

Large Queue: All jobs that require more than 100 nodes are automatically transferred to the large queue. The large queue is limited to one job per user at a time, with a requested wall-clock time limit of 300 hours.

Largemem Queue: Jobs which need large amounts of memory up to 1TB.   

The current list of resource limits can be found [here](http://hpcc.usc.edu/support/infrastructure/account-resource-limits/).


##Interactive Job Submission Example

```
[jmehring@hpc-login2 ~]$ qsub -I -l walltime=00:05:00 -l nodes=1:ppn=8
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
[jmehring@hpc1118 ~]$ cd Sources/cluster-tests/fortran
[jmehring@hpc1118 fortran]$ ./64.out
 FORTRAN SAYS: Hello World!
[jmehring@hpc1118 fortran]$ exit
logout

qsub: job 5046521.hpc-pbs.hpcc.usc.edu completed
[jmehring@hpc-login2 ~]$ 
```

##PBS Script Job
```



