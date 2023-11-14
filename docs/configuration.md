---
layout: default
title: HowTo configure your C-LAEF 1k suite
nav_order: 1
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



***paths***
Some basic paths to be used in the suite. Paths include some MACROS that are replaced in *read_config*. 
|     **key word**     |      **value**       |            **Description**           |
|----------------------|----------------------|--------------------------------------|
|    lbc_ifs           | "/ec/@STHOST@/tc/zat/tcwork/ecdiss" | Directory where operational lbc input of ECMWF-IFS data are located. The directory holds input for 927 (*Note*: At the moment Member 00 and 17 use this directory hard-coded). |
|    lbc_ens           | "/ec/@STHOST@/tc/zat/tcwork/ecdiss_ef" | Directory where operational lbc input of ECMWF-ENS are located. These files are used by all perturbed C-LAEF member. |
|    climdir           | "/ec/@STHOST@/tc/zat2/home/CLAEF_1k/CLIM/@MODEL_CYCLE@" | Location of climate files and all kind of static data. 
|    bindir            | "/ec/@STHOST@/tc/zat2/home/CLAEF_1k/BIN/@MODEL_CYCLE@" | Directory where Binaries are stored. At the moment only one location is implemented that has to provide all binaries required. |

