# Quickstart
This manual will give you all the information you need to use the cluster. You can find more guides in the documentation after this page, which will help you with more specialised issues. Or you can find some step by step tutorials you can follow, if this is easier for you to learn with.

## Introduction
HPC stands for High Performance Computing. Commonly this refers to a cluster of servers with ressources shared by multiple people. To manage resource allocation a scheduler is used that based on your definitions creates an isolated work environment to run code. As the resource is shared and not always used evenly, jobs can be queued for a while before they are run by the scheduler. No instant access to resources is quaranteed.

### How can HPC help you?
- Your work has outgrown your personal device's resources.
- Your work runs for hours or days and prevents you from using your personal device for other work.
- You do not have the right resources, like GPU, available to you in your personal device.
- You don't have the resources to aquire an own powerful computer and maintain it.

### Architecture
HPC can be built in in different configurations. This configuration is very popular and you will see it in many other HPCs

![Architecture Diagram](assets/quickstart_imgs/architecture.svg)

The HPC is build of these components:

- **Login Node**: This is the system all users interact with. If you want to interact with any of the other components, it has to be through the login node.
- **Scheduler**: This is the brain of the cluster. All your scheduling and status requests from the login node go this system. 
- **Compute Servers**: This symbolises all compute servers you could get allocations from through the scheduler. You should not access the servers directly unless you have a allocation. 
- **Shared Storage**: Most of the storage you interact with on the cluster is shared between the login node and all available compute servers. Therefore you can manage the data you need within your jobs from the login node.
- **HPC Network**: The network is shut off from the Campus network and therefore systems within it, will not be accessible to users outside of the login node.

### How does HPC work?
You will be submitting "work packages" in form of submission scripts, that define your resource requirement, an educated estimation of how long your job will run, and the workflow you want to run. Your work should run fully autonomous, this means no human interaction like GUI or console inputs. You can however, also have interactive sessions through HPC, these are useful for environment preparations, tests and debugging. 

Your work package will then be queued by the scheduler. The scheduler will decide where your job can run, based on your resource requirements described. Multiple jobs can run on one server. If there is enough resource available your job might run right away, otherwise it will be queued and run at a later time. You should be able to provide contact information, and the scheduler will keep you in the loop if your job has started, finished or failed. 

Scheduling is a complicated matter, and multiple factors play into the priority of your job, however generally, the smaller your job, the faster it will run, so it pays out to be efficient!

---

## Access the Cluster
After you got your account, to access the system, you log into a login node. The login node is the central point of access to the cluster for all users. This server is not very powerful and should therefore not be used for computational work. Any computational work should go through a job allocation on the scheduler.

### Connection Information
You have to be connected to the Campus network either via LAN, eduroam or VPN.

=== "Central"

    - **Hostname**: `headnode04.cent.gla.ac.uk`
    - **Username**: *University of Glasgow GUID*
    - **Password**: *GUID Password*

### Connecting via SSH
You will need to use `SSH` to connect to the login node and use the HPC. The simplest way to connect is by opening a console and connect using the preinstalled `SSH`utility of your device(If you are prompted for a password, it will not show up while typing):

```
ssh <username>@<hostaname>
```

We would recommend you use a SSH GUI client for regular access to the platform, as it allows you to save sessions, and `copy+paste` more easily. Example software are [PuTTY](https://www.putty.org/) and [MobaXterm](https://mobaxterm.mobatek.net/), however you can use whatever you prefer. 

---

## Data Management
Data is an important part of HPC. Where and how to store your data is important for efficient usage of the platform. 

All storage available is to be used for the duration of a your work. It is not expected to provide long term/primary storage. The data will assumed to be transient with only limited protection. As the HPC is not a primary storage solution, we recommend storing all HPC data, you can’t afford to lose in a primary, safe location like a centralised storage system provided by your school or a Team within Microsoft Teams. With each Team, you get 5TB of cloud storage. You can use Rclone to manage your data from MARS.

!!! warning

    **This is not a trusted research environment**, therefore all research data must be anonymised prior to transferring it onto the system.

### Storage Spaces

=== "Central"

    #### User Home

    |||
    |---|---|
    |**Size**|100G (quota per user)|
    |**Path**|`/mnt/home/<GUID>`|
    |**Use**|Set up your environments and store all the scripts and data you need for your personal use.|

    #### Shared User Scratch

    |||
    |---|---|
    |**Size**|~180Tb (shared between all cluster users)|
    |**Path**|`~/sharedscratch` or `/mnt/scratch/users/<GUID>`|
    |**Use**|This storage is shared between all nodes. Read and write data that you need during your jobs. Please ensure to clean up your scratch space after you are done processing your job, to make the space available for other users to use!|

    #### Local Node Scratch
    
    |||
    |---|---|
    |**Size**|700G|
    |**Path**|`~/localscratch` or `/tmp/users/<GUID>`|
    |**Use**|This storage is local to the node and can’t be accessed outside of it. Read and write here for the best possible storage performance. If you drop files into the localscratch of the login node it won’t be available to you on the compute nodes, so the moving of data has to be part of your workflow /submission script. Please ensure to clean up your scratch space after you are done processing your job, to make the space available for other users to use!|

 
### Transfer Data
To transfer data from your local machine (or another system), you can use `SSH`. You can do this either with the `scp` command:

```
scp <source file> <guid>@<hostname>:<target file>
```

Or you can use a graphical SFTP Client of choice, for example [WinSCP](https://winscp.net). Use the connection details of the login node, mentioned above to connect.

---

## Scheduler
The scheduler used is **Slurm Workload Manager**, developed by SchedMD. Slurm has a very in depth documentation themselves, which could be useful to read through, for a more in depth understanding of how this software works [Quick Start User Guide](https://slurm.schedmd.com/quickstart.html). 

The information here is the configurations you will need to know to use the University of Glasgow HPC.

### Resources
Compute servers, or also called nodes, can carry different resource configurations, to fit different workloads. For example, some servers might offer high amount of CPU, while others offer GPU resource.

=== "Central"

    The Central HPC is very heterogeneous, meaning it is comprised of a vast variety of hardware! You can  get an overview of all servers and their available resources by running the command below on  the system:

    ```
    sinfo -o "%20n %10c %20m %30G"
    ```

    ??? info

        - **CPUS:** Number of CPUs available on the node.
        - **MEMORY:** Amount of memory / RAM available on the node in MB.
        - **GRES:** GPU resources available on the node. `gpu:<type>:<amount>`.

### Partitions / Queues
Partitions, also known as queues on other scheduling systems, are used to determine which nodes you want your job to run. 

=== "Central"

    |Name|Description|
    |---|---|
    |cpu|This is the *default* partition, meaning this is chosen when no partition is specified. It contains all CPU focused servers of the Cluster.|
    |gpu|This partitions cotains all servers with GPU resources available. You can specify which type with the `--gres` parameter.|


### Parameters
You can use a whole lot of parameters to specify your resource request to the scheduler. We will cover the most common parameters here, but if you want a full overview of all available ones, you can run `man sbatch` or `man srun` when logged into the system. 

|Parameter|Example|Description|
|---|---|---|
|`-J, --job-name=<jobname>`|`--job-name="Test-Job-1"`|Specify a name for the job. The specified name will appear along with the job id number when querying running jobs on the system. The default is the supplied executable program's name.|
|`-N, --nodes=<num>`|`--nodes=2`|Number of servers you want your job to run on. Use only if your code supports parallel processing.|
|`-c, --cpus-per-task=<num>`|`--cpus-per-task=8`|Request the number of CPUs to be allocated per process. This may be useful if the job is multithreaded and requires more than one CPU per task for optimal performance.|
|`--gres=gpu:<type>:<num>`|`--gres=gpu:gtx1080:1`|Specify the type and number of GPUs for your allocation.|
|`--mem=<num><unit>`|`--mem=16G`|Specify the real memory required per node. Default units are megabytes. Different units can be specified using the suffix [K|M|G].|
|`-p, --partition=<partition_names>`|`--partition=gpu`|Request a specific partition for the resource allocation. If the job can use more than one partition, specify their names in a comma separate list. If the parameter is not set it defaults to configured default partition.|
|`-t, --time=<dd-hh-mm-ss>`|`--time 00-01:00:00`|Set a limit on the total run time of the job allocation. When the time limit is reached, each task in each job step is sent a kill signal.|
|`--mail-user=<email>`|`--mail-user="example@example.co.uk"`|Email address to send notifications to. Only University of Glasgow managed emails are accepted.|
|`--mail-type=<type>`|`--mail-type="BEGIN,END,FAIL"`|Comma separated list of event types `--mail-user` gets notified for.  Valid type values are NONE, BEGIN, END, FAIL, RE‐QUEUE, ALL|

---

## Software

---

## Support

---
