---
layout: default
title: HowTo configure your C-LAEF 1k suite
nav_order: 2
---

The config file is sperated in different sections.

***general***  
Some general settings for C-LAEF 1k suites

|     **key word**     |      **value**       |            **Description**           |
|----------------------|----------------------|--------------------------------------|
|  suite_home    | "default"   | Home directory of suite will be replace by current directory, here are your files required by ecflow. |
|  suite_name    | "claef1k"   | Name of your ecflow-suite. Choose any name you like |
|  account       | "atlaef"    | SBU account your jobs are using on ATOS |
|  schost        | "hpc"       | Where you want to submit your jobs to. Use "hpc" on ATOS |
|  sthost        | "ws2"       | Only relevant for tc-users on ATOS, not used when suite is run under 'normal' user |
|  debug         | 1           | Debug level, *not yet implemented*
|  start_date    | "Now"       | start date of your suite. "Now" means that the suite starts with current day. Use any date in YYYYMMDD format if you want to run historic cases (not yet tested) |
| end_date       | 20281231    | end date of your suite. The suite will cycle until the given date is reached, then the suite will stop |
| mode           | "research"  | If set to "oper" some additional administration tasks will be added to the suite that are not necessary (and will not work) under normal users, e.g. mirror the current date between the two file systems available under tc-users
| historic       | false       | If set true the time dependencies of the suite are removed. Hence the suite will cycle to the next day as soon as all init times are complete and the suite will progress (*not yet tested*)
| institutes     | ["gsa", "arso"] | list of institutes that should be considered in the suite. E.g. the transfer will be done to all institutes if required, post-processing could be split up, observation retrievals and archiving could be done seperately. However only transfer might be a real use case in the near future? |
| gateway        | ecaccess.zamg.ac.at | Gateway to be used for file transfer. This switch is not yet implemented and might not be used in the future |
| remote         | ment_arch    | Association to be used in file transfers. Not yet implemented and might or might not be used in the future |




***paths***  
Some basic paths to be used in the suite. Paths include some MACROS that are replaced in *read_config*. 


|     **key word**     |      **value**       |            **Description**           |
|----------------------|----------------------|--------------------------------------|
|    lbc_ifs           | "/ec/@STHOST@/tc/zat/tcwork/ecdiss" | Directory where operational lbc input of ECMWF-IFS data are located. The directory holds input for 927 (*Note*: At the moment Member 00 and 17 use this directory hard-coded). |
|    lbc_ens           | "/ec/@STHOST@/tc/zat/tcwork/ecdiss_ef" | Directory where operational lbc input of ECMWF-ENS are located. These files are used by all perturbed C-LAEF member. |
|    climdir           | "/ec/@STHOST@/tc/zat2/home/CLAEF_1k/CLIM/@MODEL_CYCLE@" | Location of climate files and all kind of static data. 
|    bindir            | "/ec/@STHOST@/tc/zat2/home/CLAEF_1k/BIN/@MODEL_CYCLE@" | Directory where Binaries are stored. At the moment only one location is implemented that has to provide all binaries required. |


***eps_setup***  
Basic setup of your ensemble.  

|     **key word**     |      **value**       |            **Description**           |
|----------------------|----------------------|--------------------------------------|
| member               |   18                 | number of member the ensemble should consist of |
| fcst_range           | {0 = 6, 3 = 3, 6 = 3, 9 = 6, 12 = 3, 15 = 3, 18 = 3, 21 = 3} | dictionary like definition of fcst_ranges, listed for every init-time seperately |
| couplf               | 1                    | coupling frequency for LBCs in hours |
| step15               | false                | 15 minute output required, should be coded more flexible |
| surf927              | true                 | Should 927surf be executed or not. |
| coupl_model          | "ENS"                | Define which model to use for LBC generation. "ENS" uses ECMWF-ENS files from lbc_ens, "DET" ECMWF-HRES from lbc_ifs | 


***assim_setup***  
Basic setup for assimilation 

|     **key word**     |      **value**       |            **Description**           |
|----------------------|----------------------|--------------------------------------|
| assimi               | true                 | should assimilation be switched on   |
| assimm               | 0                    | number of members without 3D-Var. Will be deprecated or changed to something like number of Member with randomlike setup |
| assimc               | 3                    | length of assimilation cycle in hours, hence the time interval between two initial times |
| eda_pert             | true                 | switch on/off eda in 3D-Var with randomly perturbed parameter |
| eda_unpert           | false                | switch on/off eda in 3D-Var with fixed parameter |
| seda                 | true                 | switch on/off surface eda |
| pertsurf             | true                 | switch on/off perturbation of sfx files |
| stophy               | true                 | switch on/off stochastic pyhsics |
| envar                | false                | switch on/off envar |


***model_setup***  
Basic setup for model configuration. 

|     **key word**     |      **value**       |            **Description**           |
|----------------------|----------------------|--------------------------------------|
| model_cycle          | cy46t1               | Model cycle to be used               |
| n_io_serv            | 10                   | number of CPU to be used for IO-server. 0 means, run without IO-Server |

 
***verif***  
Switch on/off Extraction of values on station location using HARP. The extraction of model data is implemented in python. Please note that the model data are written in sqlite files as Ensemble even if the suite is used to run only one determinstic model. One has to deal with that in the HARP verification part.

|     **key word**     |      **value**       |            **Description**           |
|----------------------|----------------------|--------------------------------------|
| verif_io             | {0 = true, 3 = false, 6 = false, 9 = true, 12 = false, 15 = false, 18 = false, 21 = false} | dictionary like definition on which init-times harp-io should be executed |
| verif_trans          | false                | switch on/off transfer of harp sqlite files |
| tasks_harp_param     | ["T2m","rhum2m",....] | List of surface Parameter to be extracted with harp-io |
| upper_params         | ["T", "rhum", "u", "v"] | List of parameter that should be extracted for verification on pressure level |
| level                | ["500", "700", "850"]  | List of pressure level where parameter should be extracted |


***timing***  
Times where certain tasks should be triggered. This has to be recoded in the future since there is for sure a better way how to define these times.  

|     **key word**     |      **value**       |            **Description**           |
|----------------------|----------------------|--------------------------------------|
| comp                 | "00:30"              | Time when suite is forced to complete |
| clean                | "05:00"              | Time when cleaning task for ecflow-log file is executed |
| oHH_1, oHH_02        | "0145", "0155"       | Time range from oHH_1 to oHH_2 where task to get observations is started. HH stands for initial time |
| cHH_1, cHH_02        | "02:30", "06:00"     |  _1 is the time when the check_obs tasks are started, _2 is the time when the check_main tasks are started. HH stands for initial time |


## Optional part of the config file  
In the following sections, basic settings specified above can be overwritten to make the suite more configurable   

***member_setup***    
In this section member specific settings can be given. So it is possible to change the setup for single members. The syntax is *member*.*key* = *value*

Examples:
-   **0.pertsurf = false**: Surface perturbations are swichted off for Member 0
-   **17.envar = true**: EnVAR is switched on for Member 17
-   **3.fcst_range = {0 = 9, 3= 3, 6 = 54, 9 = 32, 12 = 24, 15 = 3, 18 = 3, 21 = 3}**: Forecast ranges for Member 03 are updated. Be aware that you have to specify all init-times even if only some are changed.

***exp_setup***  
In this section settings from the *general* section of the config-files can be used or changed. 
Example:
- **arch = {gsa = true, arso = true}**: Switch on MARS archiving for institutes GSA and ARSO
- **trans = {gsa = true, arso = false}**: Switch on transfer of gribfiles to GSA but switch it off for ARSO







