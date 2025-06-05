# Slurm Job Monitoring
Slurm is saving accounting data for every job run on the system. The accounting database contains information about exit codes, resource usage, runtime and more. With this utility you can keep track of your usage of the cluster for currently running or finished jobs.

To access this information you can use the sacct command. This is a very powerful command and the full documentation can be found here sacct or you can run man sacct on the system. Here some examples:

Show all your jobs from the last 7 days:

```
sacct -u <GUID> -S $(date +%Y-%m-%d -d "-7 days") -X
```

Resource information for a specific job:

```
sacct -j <JobID> -o JobID,JobName,Partition,Account,AllocNodes,AllocTRES -X -P
```

For the -o parameter you can choose all the information listed and described here: sacct – Job-Accounting-Fields

An additional command, which bases its data from the Slurm accounting database is the seff command. This should only be used on completed jobs, or the shown data could be wrong.

seff gives you a quick overview of resources of a job including the efficiency

```
seff <JobID>
```