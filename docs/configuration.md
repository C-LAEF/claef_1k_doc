---
layout: default
title: HowTo configure your C-LAEF 1k suite
nav_order: 1
---

The config file is sperated in different sections.

***general***  

|  **key word**  |  **value**  |  **Description** |
|----------------|-------------|------------------|
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

