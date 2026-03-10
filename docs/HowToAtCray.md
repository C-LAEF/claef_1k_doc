---
layout: default
title: HowTo run C-LAEF 1k suite at Cray (local cluser)
nav_order: 1
---
## Conda
- make sure you have a working conda installation on your account

## get the repository
- Clone the C-LAEF AA repo from your own github-fork
- create the relevant conda environments:
- *conda env create --file ecflow_cray.yml*. The ecflow_cray.yml is part of the repo and can be found in *include/gsa*
- *conda env create -n oldearthkit -f earthkit_0_10_4.yml*. The earthkit.yml file is also located in *include/gsa*
- *conda activate oldearthkit* -> then install *pyproj* via conda-forge

## Start GUI and the ecflow server
- Start your ecflow-server by running the *ecflow_start.sh* script within your *ecflow* conda environment. You have to do this only the first time you set this up, or when your ecflow-server crashed.
- Command *ecflow_ui &* should open the graphical user interface of ecflow
- create your ecflow server, with its name, host, and port configuration under *Servers -> Manage Servers -> add server* Leave the category "custom user" empty.
- check in the terminal if your server is running: *ecflow_client --ping --host [crayfe1/crayfe2] --port [your_port_number]*

## Change config file:
- open claef1k/def/config_gsa.toml
- under [general], modify your email address and host/port combination you set in the previous step
- under [gsa], modify user and server that is used to fetch observations from NetApp
- if you want to perform simulations beyond the standard configuration, modify the settings in this file according to your needs
- if you want to transfer the final grib-files to NetApp you will need to adapt paths/target machine/target user... in *include/gsa/send2vvhmod.inp* and make sure you have your ssh-keys setup properly.

## Start your ecflow suite
- in the folder def/
- *python3 suite.py --config_file config_gsa.toml*

## run and monitor your suite
- usually with the ecflow_UI

For any further questions, contact Brigitta Goger, brigitta.goger@geosphere.at

