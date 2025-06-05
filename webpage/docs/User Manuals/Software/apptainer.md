# Apptainer

## About
Apptainer (formerly Singularity) simplifies the creation and execution of containers, ensuring software components are encapsulated for portability and reproducibility.

Containers are images run on a server, using base operating system components from that server. It’s similar to a virtual machine, where you can choose your OS and what packages to install within it, without compromising the host system.

## Load Module
```
module load apps/apptainer
```

## Usage
For usage information run the following command after loading the module:
```
apptainer --help
```

Below we have some easy manuals to get a first feel for containers, though we would recommend going through the more in depth Quick Start guide Apptainer offers in their documentation. This is linked in the “External Links” below.

### Run containers
On MARS you can run images you made yourself, or run images provided by colleagues or software developers. Please make sure the images you are using are from trusted sources!

You can run a container using the `apptainer run` command. The `<image>` part can either be the path to a `.sif` image you have saved locally or it can be a link to an image stored in a repository online.

```
apptainer run <options> <image>
```

As an example we will use the image hosted on the GitHub container registry by the Apptainer developers: docker://ghcr.io/apptainer/lolcow. All you need to do is provide the link and Apptainer will run the %runscript code of the image. In our example this will be a cow, telling you the current time and date:

```
[<GUID>@node1 [mars] ~]$ apptainer run docker://ghcr.io/apptainer/lolcow
 ______________________________
< Tue Feb 11 14:02:52 GMT 2025 >
 ------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

If you want to run a command outside of the workflow defined in `%runscript`, you can use the `apptainer` exec command:

```
apptainer exec <options> <image> <command>
```

This will not run the code within `%runscript` and instead run the code you provide on the command line:

```
[<GUID>@node1 [mars] ~]$ apptainer exec docker://ghcr.io/apptainer/lolcow echo "I won't do what I was made for!"
I won't do what I was made for!
[<GUID>@node1 [mars] ~]$
```

If you want to test and debug, you can run an interactive shell from within a container, using the `apptainer shell` command. To leave just use exit:

```
apptainer shell <options> <image>
[<GUID>@node1 [mars] ~]$ apptainer shell docker://ghcr.io/apptainer/lolcow
Apptainer> echo $SHELL
/bin/bash
Apptainer> echo "Hello, world!"
Hello, world!
Apptainer> exit
[<GUID>@node1 [mars] ~]$
```

All three of these commands have similar parameters. If you want to see all available options for the command run `man apptainer <run/exec/shell>` in your console. Below we explain some options we see as important:

|Option|Template|Example|Description|
|------|--------|-------|-----------|
|`-B, --bind=[]`|`-B <src>:<dest>`|`-B ~/mydata:/datadir`|Binds the local directory ~/mydata to /datadir within the container.|
|`--nv`|`--nv`|`--nv`|Enables Nvidia support. Use this option when working on GPU-servers to ensure you have the GPU available within your container.|
