---
layout: default
title: Directory structure of CLAEF-1k
nav_order: 1
---

If you clone your C-LAEF 1k from github the following directories are created:

***def***
Includes the config example file and the python code to generate and start the C-LAEF 1k suite. 
-   config.toml: Example config file that can be used as starting point to generate your own config file. This example should be kept up to date with any development done to the C-LAEF 1k code
-   suite.py: Main suite builder script. It calls *read_config.py* and builts the suite based on the settings defined in *config.toml*. The suite definition file is created and the suite is submitted to ecflow by default.
-   read_config.py: Function that reads the config and does some default setting depending on which user runs the suite (normal user or tc-user) 

***scripts***
Location of the ecf-scripts (shell scripts) of the tasks of the suite. 

***includes***
Location of include files. There are several ecflow specific files *head.h*, *tail.h* as well as suite related include files like:
-   sbatch.h: Slurm header for the ecflow-tasks. Most of the directives are replaced by ecflow variables
-   paths.h: Path settings for the ecflow tasks. 
-   ompi.h: Loads the required modules to run the executables
-   mpi.h: Sets environment variables for the scripts. Not only mpi-related though.
-   job_geometry.h: Sets variables related to the job-geometry (NPROC, NPRGPEW, NPRGPNS, and alike...)