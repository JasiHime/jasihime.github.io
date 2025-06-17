#PBS to Slurm Cheat Sheet
|Purpose|PBS|Slurm|
|---|---|---|
|Submit a job via script|`qsub myScript.sh`|`sbatch myScript.sh`|
|Cancel a job|`qdel <JobID>`|`scancel <JobID>`|
|List all jobs|qstat|squeue|
|List all your jobs|`qstat -u <GUID>`|`squeue -u <GUID>`|
|Advanced job information|`qstat -f <JobID>`|`sacct -j <JobID>`|
