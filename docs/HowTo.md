---
layout: default
title: HowTo run C-LAEF 1k suite
nav_order: 1
---

## Get C-LAEF 1k from github

-   Create a fork of https://github.com/C-LAEF/claef_1k on github
-   Clone the repository from your fork on ATOS
-   The default branch is the esuite-branch. This is where developments should be merged into. 

On ATOS you have to make sure that your ecflow-server is setup by ECMWF staff before you can start an ecflow suite. Run the following commands on ATOS:
- *export ECF_HOST=ecflow-gen-[USER]-001*
- *export ECF_PORT=3141*
If you don't know whether your ecflow-server is running do:  
*ecflow-client --ping*

If you get an error your ecflow-server has not been set up or is down -> create a Ticket and ask ECMWF staff to start your ecflow-server


## How C-LAEF 1k can be configured
Please be aware that making C-LAEF 1k user friendly and configurable is 'work in progress' and not all configurations might work out of the box.
Go into directory *def* and create a config-file in toml format (see the documentation of the config-file for details). An example file is given in config.toml.

*Health warning for new suite names:*  
There is a missing feature to upload a new suite to ecflow at the moment. Before you can start a new suite you have to edit one line in *suite.py*. Go to line 178 and uncomment the following line:  
\# ci.load("{0}.def".format(self.config["general"]["suite_name"]))  
If your suite is loaded the first time you have to comment this again.

Once you have created your config-file run from your top repository level (double dash for options*):  
-> python3 def/suite.py --config_file [PATH_TO_YOUR_CONFIG.toml]

If you do not specify a config_file the code will look for */home/USER/CLAEF_1k/def/config.toml*.

Your setting are printed and your suite will be loaded in ecflow. The suite is suspended by default to allow you to check the suite in ecflow_ui. To start the whole suite resume the suspended task on top level (named like the suite_name you specified in your configurations).  
Every run family starts with a *start_run* task that is suspended by default. You have to resume them manually or for a quasi-operational suite an external script must be implemented that resumes that task at a given time.
Each initial time (RUN_XX) has another dependency in the task getobs. There is by default a time dependency (defined in config[timings]) and should be executed manually for case studies. 

## How C-LAEF 1k ecflow suite is built

*suite.py* reads *read_config.py* which reads the config file given, and creates the config-dictionary used by *suite.py*. *read_config.py* also does some default setting and modifies the setup if the user specifies something in the optional sections *member_setup* and *exp_setup*. 
