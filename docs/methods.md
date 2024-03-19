---
layout: default
title: Documentation how features are implemented
nav_order: 2
---

This document is to keep track how different features/methods/etc. are implemented or setup in C-LAEF 1k


# Latent heat Nudging

Use INCA rain Analysis or forecasts in the First period of 001 to nudge forecast against it.
In C-Laef 1k we can use INCA Analysis Up to 90 Minutes after init time as getobs Starts 1:45h after init.

For the time beeing we use 15minutes precipitation analysis. As CLAEF 1k Runs with a model time step of 45s WE can use INCA precipitation every 3Minutes. To achieve that we scale INCA by 0.2 to mimic 3Minute precipitation from 15 Minute values. 
In RUC a more sophisticated method ist used where 5min INCA Analysis are combined with the 15minute values. The amounts are taken from 15 Minute Analysis and the temporal distribution from 5minute values. The reasoning for this procedure ist that 5minute Analysis tend to overestimate the prec amounts since they are mostly based on Radar Data . For CLaef 1k this method ist more difficult to implement due to the model time step that doesn't allow 5 Minute nudging. Hence for the start we use only 15min INCA Analysis and scale these files down to 3m. The *getobs* task starts 1:45h after init time, so 15min INCA Analysis up to 1:30h after init should be available and are retrieved. This allows a nudging in 3 minutes intervals up to +90minutes during 001.

#### The Implementation is done as follows:
The way it is implemented in AROME-RUC is way too slow for C-LAEF 1k so some adaptions and optimizations are implemented for C-LAEF 1k. To get rid of the computational costly interpolation from INCA to AROME/C-LAEF 1k grid, a mapping file is used that holds the indices of INCA grid and the according indices of the C-LAEF 1k grid. This was created by interrrobs33.F90 routine, based on the version used in AROME-RUC, with only a small adaptation to write out the required values at the end. As input it requires a INCA-precipitation grib file (could be any), the INCA-cooridates file (to be found in /ec/ws2/tc/zat2/home/claef1k/CLIM/cy46t1/LHN/INCAPLUSCOORD) and a text file with the C-LAEF 1k grid point locations (/ec/ws2/tc/zat2/home/claef1k/CLIM/cy46t1/LHN/CLAEFCOORD_format.txt). The latter was created with *epygram* using the following command (make sure you also extract the extension zone!).  
*epy_conv.py -F SURFTEMPERATURE -z 'CIE' -o geo --geopoints_cols LAT,LON,DATE,RUN,VALUE /scratch/kmw/lhn/INPUT.fa*

Now the *lhn_prep* task can be run by linking the INCA-grid file to *fort.11*, the C-LAEF 1k grid file to *fort.13* and run interrrobs33. This results in an output text file holding the C-LAEF grid index, the required INCA grid indices and the weighting factor to interpolate the precipitation from INCA to C-LAEF 1k. Not that this mapping file has to be re-created if any of the input grids change (resolution, domain size), or if a different smoothing value is chosen. 
You might remove the values outside the INCA-domain from this file by removing all lines with 0.000 weighting factor ('eg. by doing *grep -v "0.00000"). The resulting mapping file that is used in C-LAEF 1k is /ec/ws2/tc/zat2/home/claef1k/CLIM/cy46t1/LHN/MAP_INCA_CLAEF1k_smooth3.txt.

The maprrobs routine also reads a dummy fa-file where all Rain-fields are set to -999.99 (can be created by /home/kmw/CLAEF_1k/tools/create_emptyrrobs.py). This dummy file is read by maprrobs and the interpolated precipitation fields are written to the S0??RAIN fields specified in the namelist.

### Implementation in C-LAEF 1k suite
The file include/observations/lhn.config holds the latent heat nudging specific settings. 
In getobs the INCA precipitation analysis with 15 minute resolution are fetched from GeoSphere
In lhn_prep the Interpolation from INCA grb2 files to C-LAEF 1k grid is done and the Fields are written to ${NUDGPREPDIR}/INPUT_LHN.fa. The precipitation fields are written to S0??RAIN levels ordered by time which is important when the file is written by 001.



# GNSS 
To use GNSS observations with VARBC the following steps are required.
### Create a GNSS whitelist
This is required to select stations that are trustworthy and can be used in 3D-Var later. To create the whitelist  
1) Blacklist GNSS in LISTE_LOC3D, so bator sets the appropriate flag to GNSS observations. This can be done by adding the following line to LISTE_LOC3D 
*N  1 110              128*
2) Run a period of at least 10 days and save the odb created by screening (ECMA_screen.tar)
3) Run Mandalay on the saved odb's. An example script can be found on ATOS under: */home/kmw/CLAEF_1k/MANDALAY/scripts/mandalay.sh* which uses the binary */ec/ws2/tc/zat/home/PACKS/cy46_user_mandalay.01.OMPIIFC2104.x/bin/MANDALAY*
4) Run the hungarian analysis tool located in */home/kmw/GNSS_WHITELIST/GNSS_whitelist* which first copies all mandalay output to the working directory with get_list_ecma_batch.sh. Then copy the select_gpssol.x binary to the working directory and run it. The output is a whitelist of trushtworthy GNSS stations and the according BIAS.

### Warm up VARBC
1) Code changes are required in mf_blacklist.b, upecma.F90 and obsop_gps_surface.F90. See */ec/ws2/tc/zat/home/PACKS/cy46_development.01.OMPIIFC2104.x/* in branch passive_gnss, see also https://www.rclace.eu/forum/viewtopic.php?p=2323&hilit=GNSS#p2323
2) The above mentioned change in LISTE_LOC3D has to be reverted. 
3) Set in screening namelist: 
*NAMVARBC_SFCOBS
  LBC_SFCOBS_APD=.T.,   !GNSS Varbc
  LCOLDSTART_SFCOBS=.T.,*

