---
layout: default
title: Documentation how features are implemented
nav_order: 2
---

This document is to keep track how different features/methods/etc. are implemented or setup in C-LAEF 1k


##Latent heat Nudging

Use INCA rain Analysis or forecasts in the First period of 001 to nudge forecast against it.
In C-Laef 1k WE can use INCA Analysis Up to 90 Minutes after init time as getobs Starts 1:45h after init.

For the time beeing WE use 15minutes precipitation analysis. As CLAEF 1k Runs with a model time step of 45s WE can use INCA precipitation every 3Minutes. To achieve that WE scale INCA by 0.2 to mimic 3Minute precipitation from 15 Minute values. 
In RUC a more sophisticated method ist used where 5min INCA Analysis are combined with the 15minute values. The amounts are taken from 15 Minute Analysis and the temporal distribution from 5minute values. The reasoning for this procedure ist that 5minute Analysis tend to overestimate the prec amounts since they are mostly based on Radar Data . For CLaef 1k this method ist mire difficult to implement due to the model time step that doesn't allow 5 Minute nudging.

The Implementation ist done as follows:
Get asciifile with INCA coordinates
Create ascii File with Aromen coordinates (epy_conv.py Format Geo)
Create an FA File on modeel grid where rain fields are Set to -999.99
Get inca observations from GSA.
In include/observatins/lhn.config configuration of lhn IS defined. A Switch for using 5min INCA ist included but hast nö functionality yet.
After getobs the Script lhnprep ist triggered which does the scaling of 15 min precipitation to the Timeresolution given in the config. After that the Interpolation of inca prec to Aromen grid ist done in interrrobs33 binary. This reads the inca coord File and the Arome coord file as well AS the inca grib and the FA File with the Missing values. The prec field ist interpolated to Aromen grid with nearest neighbour and some smoothing, if specified. The interpolated field ist written in S0??Rain field of the fa-file.
Last step is a blendur which writes the
