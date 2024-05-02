---
layout: default
title: Notes on operational setup
nav_order: 6
---

The operational suite has some addtional families that are not required if run under personal user in research mode.


## special families/tasks

### admin
Especially the *admin*-family requires some further explanation. The family consists of the following tasks:

-   **cleanlog**: runs once per months at a specified day and time and cleans the ecflow-logfile 
-   **complete**: runs once per night and forces the *runs* family to complete. Hence if any job is aborted, or did not run as expected the family is set to complete and the date can cycle, such that the suite continues to run.
-   **operator**: This is a family that includes a task that allows operators, either at ECMWF or from our maintenance team to move the suite from one file system to another (ws1/ws2 on ATOS). ***ATTENTION***: Take care when this task is run. In the current setup the last ECMWF-ENS run triggers lbc production of two runs (eg. 00 UTC ECMWF-ENS is available and lbc production for CLAEF-1k 06/09 UTC are initialized). This task should be run just before a new ECMWF-ENS run arrives and *not* between two C-LAEF 1k runs that use the same ECMWF-ENS LBCs. If this is done the 927 output for the second run is not yet mirrord to the second file system and the next run will abort.  
-   **dummy3/dummy4**: Two tasks that regularely produces headaches.... Since CLAEF-1k 00 and 03 UTC runs are coupled to 18 UTC of ECMWF-ENS the change of date can be a bit cumbersome. In operations the CLAEF-1k suite changes date when the **complete** task runs, thus at 00:15UTC. 18 UTC run of ECMWF-ENS is available at approx. 00:50UTC under normal circumstances. However there were rare cases where 18 UTC ECMWF-ENS run was available and triggered before 00:15 UTC, when 00/03 UTC runs of CLAEF-1k suite are in a *complete* state from the day before. To avoid such cases 18 UTC run of ECMWF-ENS triggers task **dummy3** which changes state from suspended to complete. **dummy4** has two dependencies, one time dependency by cron (00:20 UTC, hence after the suite is cycled by **complete**) and a trigger that waits until **dummy3** is complete. **dummy4** finally starts the lbc production for runs 00/03 UTC of C-LAEF 1k and assures that this is not triggered before 00:20 UTC.  



### mirror task
At the end of every *RUN_XX* family a mirror tasks synchronizes data between the two tc-filesystems ws1/ws2. This ensures that the suite continues to run in case the file system is switched by the operators.  

## External scripts
If a new ECMWF-ENS run is available the lbc-production for C-LAEF 1k is triggered via an ecaccess-event. This is done by an external script since direct triggering of an ecflow-task by an ecaccess-event is technically not possible. The external script that triggers the operational suite are *include/ez_trigger_1km.sh* and *include/ez_trigger_1km_2.sh* respectively. The two scripts are identical except for the used file system. For normal ECMWF users only one script is required. 