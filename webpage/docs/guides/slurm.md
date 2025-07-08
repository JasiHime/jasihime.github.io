# Slurm
This guide will help you familiarise yourself with Slurm and its utilities. It will give you helpful code snippets you can use on all Slurm systems you interact with.

## Get Cluster Information
In this section we will show you how to get information about the scheduler through different slurm queries. 

### Resources
This command will show you an output of all servers  in the cluster with information about the amount or CPUs, amount of Memory (in MB) and if and what kind of GPU is available: 

```
sinfo -o "%20n %10c %20m %30G"
```

### Partitions
To get an overview of all available partitions, including their state, node count, and CPU + time restrictions use this command:

```
sinfo -o "%20P %10a %10D %20B %20l "
```

If you want in in depth overview of a specific partition, you can use `scontrol`. This output is more tachnical:

```
scontrol show partition <partition_name>
```

### Usage
If you want to get an overview of the usage of the platform, you can use the following command. The letters A/I/O/T stands for **A**llocated/**I**dle/**O**ther/**T**otal. Other usually means resource is down.

```
sinfo -O partition,nodelist,cpusstate,gres,gresused,nodeai
```

RCaaS has a script on all their managed systems called `load`, that shows you the usage of the platform per resource. You can either choose `cpu`, `gpu` or `mem`. The output is a bar visualising the usage percentage of the resource defined. 

```
load <resource>
```

## Interactive Job
Interactive jobs are great to install software, prepare your environment or debug your script. For any serious or larger work, please use batch jobs.

Get an allocation with `srun` and open an interactive bash shell with the parameter `--pty bash`. You can use any other [slurm parameters](../references/slurm-parameters.md), to define your allocation, but this parameter has to be the last one. If you require access to a GUI, you can use the `--x11` parameter.

```
srun <slurm_parameters> --pty bash
```

You are not guarateed an allocation right away! If the cluster is busy, you might be waiting hours for your interactive shell to start.

If due to a network issue you lose access to this console, there is no way to get access back, this is why it is a bad idea to use it for longer running jobs! If you lose access and the job is still scheduled to run for a long amount of time, please kill it, to make the resource available for other users again.

## Batch Job Submission
Batch job submission is the way the cluster is ideally used. You use a "Submission Script" to submit your work to the cluster. This script is usually a `bash` script containing your job specification, environment setup and then your work. Example scripts can  be found here [Submission Script Templates](../references/submission-script-templates.md).

In this example here, we are creating a directory for our job, move into it and run a python  script:

```
#!/bin/bash -l
...
############# MY CODE ############# 
mkdir ~/work/job_$SLURM_JOBID
cd ~/work/job_$SLURM_JOBID
python3 myPYthonCalcs.py
```

To submit this to the scheduler we use the `sbatch` utility, followed by the path to the submission script:

```
sbatch myScript.sh
```

You should get a message in return with a **JobID**. This number is unique to every job on the system and cen be used to manage or monitor your job.

## Job Management
### Modify Job
After scheduling your job you still have limited options to modify your job.  If your job has not started yet, you are able to change almost all parameters. If it is running already, a lot of things will be locked the way it is.

To modify the job use the `scontrol` utility. If run `scontrol show JobID=<JobID>` you see all available parameters. Here an example where we lower the runtime limit to 3 days:

```
scontrol update JobID=<JobID> TimeLimit=3-00:00:00
```

### Delete Job
To delete a job you can use the `scancel` utility followed by your JobID.

```
scancel <JobID>
```

## Job Monitoring
You have different ways to keep track of your jobs, wether they are done or still running

### Actively Running jobs
To get a list of all running jobs on the platform you can use the `squeue` utility. You can filter by user using the `-u` parameter or by JobID using `-j`:

```
squeue -u <guid> -j <JobID>
```

### Historic Job Analysis
Slurm keeps a database with information of all jobs run using the system. To access this data, you can use the `sacct` command.  Use the `-o` parameter followed by a list of Job-Accounting-Fields. A list of all available Job-Accounting-Fields can be found here: [sacct manual](https://slurm.schedmd.com/sacct.html#SECTION_Job-Accounting-Fields).

Per default you will get information for each job step in your output. If you don^t want more than one line for each of your jobs, you can use the `-X` parameter.

You can  get information for a specific job  using the `-j` parameter, folowed by your JobID:

```
sacct -X -j <JobID> -o JobID,JobName,User,Submit,Start,End
```

Or more broadly all the jobs run by your user, using the `-u` parameter with your GUID:

```
sacct -X -u <guid> -o JobID,JobName,User,Submit,Start,End
```

You can curther restrics your outous with different parameters. One of the most helpful ones are the `--starttime` and `--endtime` with these you can  define in what period you want to lookup jobs for. If you only specify `--starttime`, the endtime will  be set to NOW. Here an example, if you wanted to see  all your jobs in the last 7 days:

```
sacct -X -u <guid> --starttime $(date -d "-7days" +%Y-%m-%d) -o JobID,JobName,User,Submit,Start,End
```

## Job Efficiency