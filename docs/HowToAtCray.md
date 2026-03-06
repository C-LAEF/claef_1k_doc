---
layout: default
title: HowTo run C-LAEF 1k suite at Cray (local cluser)
nav_order: 1
---
## Conda
- make sure you have a working conda installation on your account

## get the repository
- Clone or fork the repo from https://github.com/FlorianW-ZAMG/claef_1k
- create the relevant conda environments:
- *conda env create --file claefaa_cray.yml -n claef*
- *conda install -n oldearthkit earthkit_0_10_4.yml*
- *conda activate oldearthkit* -> then install *pyproj* via conda-forge

## Start GUI and the ecflow server
- Command *ecflow_ui &* should open the graphical user interface of ecflow
- create your ecflow server, with its name, host, and port configuration under *Servers -> Manage Servers -> add server*
- check in the terminal if your server is running: *ecflow_client --ping --host [crayfe1/crayfe2] --port [your_port_number]*

## Change config file:
- open claef1k/def/config_gsa.toml
- under [general], modify your email address and host/port combination you set in the previous step
- under [gsa], modify user and server that is used to fetch observations from NetApp
- if you want to perform simulations beyond the standard configuration, modify the settings in this file according to your needs

## first guess file
- to perform simulations, you'll need a first guess file.
- create the relevant folder structure in your claef1k repo: */DATA/20260210/21/MEM_00/001/*, adjusting date/time.
- you will need the first guess from the simulation 3h prior to your initialization., e.g., if yout simulation starts at 00UTC, you will need the FG from 21UTC from the previous day.
- Copy the file from fweidle: */lustre/scratch/nwp/fweidle/claef1k/DATA/20260210/21/MEM_00/001/ICMSHAROM+0003:00**

## Start your ecflow suite
- in the folder def/
- *python3 suite.py --config_file config_gsa.toml*

## run and monitor your suite
- usually with the ecflow_UI

For any further questions, contact Brigitta Goger, brigitta.goger@geosphere.at

