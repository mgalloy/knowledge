# Portable Batch System (PBS)

Use PBS commands in a shell script to submit jobs to a queue on a
supercomputer.

## Documentation and examples

See man pages on ***beach***:

	$ man qsub
	$ man pbs
	$ man pbs_resources

Some helpful pages:

* HPC@LSU documentation: [http://www.hpc.lsu.edu/docs/pbs.php](http://www.hpc.lsu.edu/docs/pbs.php)
* [http://www.nas.nasa.gov/hecc/support/kb/Commonly-Used-PBS-Commands_174.html](http://www.nas.nasa.gov/hecc/support/kb/Commonly-Used-PBS-Commands_174.html)
* [http://www.nas.nasa.gov/hecc/support/kb/Commonly-Used-QSUB-Options-in-PBS-Scripts-or-in-the-QSUB-Command-Line_175.html](http://www.nas.nasa.gov/hecc/support/kb/Commonly-Used-QSUB-Options-in-PBS-Scripts-or-in-the-QSUB-Command-Line_175.html)

## Submit a job

Use the `qsub` command:

	$ qsub foo.sh

Here, **foo.sh** is a bash shell script
containing the call to the program to execute.

* `-I` invokes interactive mode. This puts you directly onto a compute
  node, so you can execute commands and see results.
* `-q [queue]` submits a job to a particular queue.

## Check job status

Use `qstat` to show all jobs:

	$ qstat

* `-q` to check a particular queue
* `-u mapi8461` to check my jobs
* `-f` for detailed output

Use `checkjob` on a particular job:

	$ checkjob [job_id]

Use `showq` for an alternate listing of all jobs.

	$ showq

## Set the priority of a job

Use the `-p` option to `qsub`:

	$ qsub -p -50 foo.sh

The priority must be an integer between -1024 and 1023,
with lower values corresponding to lower priorities.

The `qalter` command can be used to modify properties of a job
after submission, but before execution:

	$ qalter -p -1000 [job_id]

## Email

Send an email on job start (`-b`), finish (`-a`) and error (`-e`)
with PBS commands embedded in the header of the run script **foo.sh**.

	#PBS -m -bae
	#PBS -M mark.piper@colorado.edu

## Moar power

By default,
only one node with one processor is allocated per job.
To get more compute power, use PBS commands.

Request all eight processors on a node:

	#PBS -l nodes=1:ppn=8

Request two nodes, but only four processors on each:

	#PBS -l nodes=2:ppn=4

## Specify a node for a job

Probably shouldn't do this since it's the scheduler's job,
but,
to specify a particular node on which to run a job,
use the `nodes` keyword:

    #PBS -l nodes=compute-2-4

## Which nodes belong to which queues?

Also,

* What jobs are running on which nodes?
* How much memory does a node have?

To answer these questions (and more!),
use the `pbsnodes` command:

    $ pbsnodes

You can also check a particular node:

```
$ pbsnodes compute-0-2
compute-0-2
     state = free
     np = 8
     properties = debug
     ntype = cluster
     status = rectime=1484853611,varattr=,jobs=,state=free,size=217642636kb:219059416kb,netload=28526413744,gres=,loadave=0.01,ncpus=8,physmem=16463720kb,availmem=16383020kb,totmem=17487716kb,idletime=217,nusers=0,nsessions=0,uname=Linux compute-0-2.local 2.6.32-504.el6.x86_64 #1 SMP Wed Oct 15 04:27:16 UTC 2014 x86_64,opsys=linux
     mom_service_port = 15002
     mom_manager_port = 15003
```

## Job chaining

Calling a PBS job from another PBS job.
See
[https://www.nics.tennessee.edu/computing-resources/running-jobs/job-chaining](https://www.nics.tennessee.edu/computing-resources/running-jobs/job-chaining).

