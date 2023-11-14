---
layout: default
title: HowTo run C-LAEF 1k suite
nav_order: 1
---

# Get C-LAEF 1k from github

-   Create a fork of https://github.com/C-LAEF/claef_1k on github
-   Clone the repository from your fork on ATOS
-   The default branch is the esuite-branch. This is where developments should be merged into. 

On ATOS you have to make sure that your ecflow-server is setup by ECMWF staff before you can start an ecflow suite. Run the following commands on ATOS:
- export ECF_HOST=ecflow-gen-[USER]-001
- export ECF_PORT=3141
If you don't know whether your ecflow-server is running do:
ecflow-client --ping

If you get an error your ecflow-server has not been set up or is down -> create a Ticket and ask ECMWF staff to start your ecflow-server


# How C-LAEF 1k can be configured
Please be aware that making C-LAEF 1k user friendly and configurable is 'work in progress' and not all configurations might work out of the box.
Go into directory def and create a config-file in toml format (see the documentation of the config-file for details). An example file is given in config.toml.
Once you have created your config-file run
-> python3 suite.py

Your setting are printed and your suite will be loaded in ecflow.