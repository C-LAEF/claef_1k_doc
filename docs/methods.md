---
layout: default
title: Documentation how features are implemented
nav_order: 2
---

This document is to keep track how different features/methods/etc. are implemented or setup in C-LAEF 1k


##Latent heat Nudging

Use INCA rain Analysis or forecasts in the First period of 001 to nudge forecast against it.
In C-Laef 1k we can use INCA Analysis Up to 90 Minutes after init time as getobs Starts 1:45h after init.

For the time beeing we use 15minutes precipitation analysis. As CLAEF 1k Runs with a model time step of 45s WE can use INCA precipitation every 3Minutes. To achieve that we scale INCA by 0.2 to mimic 3Minute precipitation from 15 Minute values. 
In RUC a more sophisticated method ist used where 5min INCA Analysis are combined with the 15minute values. The amounts are taken from 15 Minute Analysis and the temporal distribution from 5minute values. The reasoning for this procedure ist that 5minute Analysis tend to overestimate the prec amounts since they are mostly based on Radar Data . For CLaef 1k this method ist more difficult to implement due to the model time step that doesn't allow 5 Minute nudging. Hence for the start we use only 15min INCA Analysis and scale these files down to 3m. The *getobs* task starts 1:45h after init time, so 15min INCA Analysis up to 1:30h after init should be available and are retrieved. This allows a nudging in 3 minutes intervals up to +90minutes during 001.

####The Implementation is done as follows:
The way it is implemented in AROME-RUC is way too slow for C-LAEF 1k so some adaptions and optimizations are implemented for C-LAEF 1k. To get rid of the computational costly interpolation from INCA to AROME/C-LAEF 1k grid, a mapping file is used that holds the indices of INCA grid and the according indices of the C-LAEF 1k grid. This was created by the original interrrobs33.F90 routine with only a small adaptation to write out the required values at the end. As input it requires a INCA-precipitation grib file (could be any), the INCA-cooridates file (to be found in /ec/ws2/tc/zat2/home/claef1k/CLIM/cy46t1/LHN/INCAPLUSCOORD) and a text file with the C-LAEF 1k grid point locations (/ec/ws2/tc/zat2/home/claef1k/CLIM/cy46t1/LHN/CLAEFCOORD_format.txt). The latter was created with *epygram* using the following command (make sure you also extract the extension zone!).  
*epy_conv.py -F SURFTEMPERATURE -z 'CIE' -o geo --geopoints_cols LAT,LON,DATE,RUN,VALUE /scratch/kmw/lhn/INPUT.fa*

Now the *lhn_prep* task can be run by linking the INCA-grid file to *fort.11*, the C-LAEF 1k grid file to *fort.13* and run interrrobs33 if you use the interrrobs33.F90_printgrid version. This results in an output text file holding the C-LAEF grid index, the required INCA grid indices and the weighting factor to interpolate the precipitation from INCA to C-LAEF 1k. Not that this mapping file has to be re-created if any of the input grids change (resolution, domain size), or if a different smoothing value is chosen. 
You might remove the values outside the INCA-domain from this file by removing all lines with 0.000 weighting factor ('eg. by doing *grep -v "0.00000"). The resulting mapping file that is used in C-LAEF 1k is /ec/ws2/tc/zat2/home/claef1k/CLIM/cy46t1/LHN/MAP_INCA_CLAEF1k_smooth3.txt.

The interrrobs33 routine also reads a dummy fa-file where all Rain-fields are set to -999.99 (can be created by /home/kmw/CLAEF_1k/tools/create_emptyrrobs.py). This dummy file is read by interrrobs33 and the interpolated precipitation fields are written to the S0??RAIN fields specified in the namelist.

##Implementation in C-LAEF 1k suite
The file include/observations/lhn.config holds the latent heat nudging specific settings. 
In getobs the INCA precipitation analysis with 15 minute resolution are fetched from GeoSphere
In lhn_prep the Interpolation from INCA grb2 files to C-LAEF 1k grid is done and the Fields are written to ${NUDGPREPDIR}/INPUT_LHN.fa. The precipitation fields are written to S0??RAIN levels ordered by time which is important when the file is written by 001.

