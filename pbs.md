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

You can also request a specific compute node:

	#PBS -l nodes=compute-2-4:ppn=2

## Job chaining

Calling a PBS job from another PBS job.
See
[https://www.nics.tennessee.edu/computing-resources/running-jobs/job-chaining](https://www.nics.tennessee.edu/computing-resources/running-jobs/job-chaining).

