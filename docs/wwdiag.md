---
layout: default
title: ww diagnostics
nav_order: 7
---

## Background

This diagnostics has been developed at GeoSphere Austria by Wastl and Wittmann (ww) to improve 2m forecasts of AROME in complex terrain such as the Alps. The idea was to take the 5 different canopy levels (switched on operationally at the moment of the development of ww diagnostics in 2020) and modify the height of the screening level accordingly (in default it is the second canopy level which is at 2m). Verification has shown that the performance of this standard canopy diagnostics is strongly depending on the region. Therefore, to differ between valley and mountains, the IFAC has been introduced into PGD. It is a number between -1 (valley) and +1 (mountain), 0 indicates flatland. The idea was to lower the screening level in Alpine valleys and to increase it on the mountain peaks. Since this is not sufficient in clear sky nights in the Alpine valleys, an additional trimming towards surface temperature has been introduced. In case of extreme events (heat waves, extreme cold spells) a so called „joker“ to directly modify the temperature output has also been installed.   
With the upgrade of C-LAEF to 1k in 2024, the ww diagnostics has been extended to a usage without canopy scheme. In this case the screening level height (default is 2m for temperature and humidity) is adapted.

## Methodology (Canopy scheme on)
The ww diagnostics is activated in the namelist block NAM_DIAG_ISBAN of the surfex namelist by choosing N2MTG=1 and by setting LDIAGSCREENWW=.T. in NAMPHY of the 001 namelist. The main routine dealing with ww diagnostics is surfex/SURFEX/coupling_isba_canopyn.F90. 
The ww diagnostics has been developed based on the 3 requirements explained above.   

1.)	The first part is the determination of the screening level in each IFAC class defined by namelist settings (see namelist description in the section below). This is done by a linear interpolation between the defined level at the beginning of the forecast (e.g. namelist switch ZLEVEL_DAY_START) and the end (ZLEVEL_DAY_END). The transition between the level at the beginning and the end is based on a slope parameter defined in the namelist (XLEVEL_K_NIGHT, XLEVEL_K_DAY). 1 means that the end level is reached at the end of the forecast, 2 means that it is reached after the half of the forecasting range, etc. 
There are seperate namelist switches for day and night and for each of the 3 IFAC classes. The levels can only be chosen between 1 (lowest canopy level at 0.5m) and 5 (highest canopy level = lowest model level, 5m at the moment). The values are clipped if they are outside these intervals. The distinction between day and night is done by a radiation check in apl_arome. The result is stored in the ZTINV variable which is passed to coupling_isba_canopy.F90. It is 0 during the day and 1 in the night. To avoid abrupt jumps in the transition between day and night, we have implemented a temporal smoothing which can be steered by namelist switch XSMOOTH_DAY_NIGHT (in seconds). 
The screening level temperature is then based on the calcualted level. E.g. if the outcome of the definition is 1.3, the sceening level temperature is a combination of the temperature at canopy level 1 (70% weight) and the one at canopy level 2 (30% weight). Q is treated accordingly.
It turned out that this defintion of the screening level temperature is not sufficient to reach observed nocturnal temperatures in the valleys during clear sky nights. Therefore a surface trimming has been installed.

2.)	During the night (no radiation), in case of stable conditions (temperature at level 75 is lower than at level 90) and no clouds (all these criteria are checked in apl_arome.F90 and passed to coupling_isba_canopyn.F90 as variable ZTINV) the temperature can be pushed towards the surface temperature (ZACT_TRIM_TSURF). E.g. if ZACT_TRIM_TSURF is 0.3 then the weight of the surface temperature is 30% and the one of the level defined in 1. is 70%, if the ZTINV criteria is fulfilled. Also for the trimming towards the surface a temporal smoothing is applied (defined by XSMOOTH_DAY_NIGHT).

3.)	For extreme cases (very shallow inversion layer with strong temperature differences) we have installed a so called „joker“ function. This joker is very simple, it reduces directly the sceening level temperature by a number defined in the namelist. Analogous to 1. also here a linear interpolation between the „joker“ value at the beginning of the forecast (e.g. XMODIF_TSURF_START) and the end (XMODIF_TSURF_START) is made. The transition between the value at the beginning and the end is based on a slope parameter defined in the namelist (XLEVEL_K_NIGHT). 1 means that the end level is reached at the end of the forecast, 2 means till the half of the forecasting range, etc.

A similar „joker“ has also been implemented for summer heat waves to avoid unrealistic temperatures above 40 degrees. It is constructed in a similar way to the TSURF „joker“ above with the namelist switches (XMODIF_TLEV_START and XMODIF_TLEV_END) and a slope defined by XLEVEL_K_DAY. The TLEV „joker“ is only active in case of radiation (ZTINV criterion) and if the temperature exceedes a threshold defined by (XMODIF_TLEV_THRES). To avoid unrealistic jumps when the temperature exceeds this threshold, an additional transition has been installed for this TLEV „joker“. With XMODIF_TLEV_K you define a linear function between XMODIF_TLEV_THRES and XMODIF_TLEV_THRES+ XMODIF_TLEV_K. This means e.g. that the temperature will be decreased with full XMODIF_TLEV at a temperature of XMODIF_TLEV_THRES+ XMODIF_TLEV_K.

## Methodology (Canopy scheme off)
The original ww diagnostics has been developed for a situation when the canopy scheme is switched on, but with the upgrade of C-LAEF to 1k we wanted to get rid of this canopy scheme and therefore it has been coded into the routine surfex/SURFEX/diag_inline_isban.F90 as well. It is activated by setting N2MTG=1 in the block NAM_DIAG_ISBAN. 
The methodology is pretty much the same as above, the only difference is part 1. Here we do not adapt the canopy levels as before, but we adapt directly the screening level height ZH. This ZH is then passed to the according tq interpolation routine (depending on N2M in NAM_DIAG_SURFN). This means that the ww diagnostics can be used together with Paulsen (N2M=1), Gelyn (N2M=2) or Dian (N2M=2) diagnostics. The namelist switches stay the same, but the level can be chosen between 0 (surface) and the height oft he lowest model levels (5 at the moment). The trimming against Tsurf is done only in case of ZTINV criterion is fulfilled (radiation, inversion, clouds) and is an additional transition towards the surface temperature. 
The „jokers“ are working in the same way as in the canopy version.

## Namelist switches in block NAMPHY of the 001 namelist:
LDIAGSCREENWW=.T.,

## Namelist switches in block NAM_DIAG_ISBAN of the surfex namelist:
Most switches are defined as a field with 3 arguments (valley, flatland, moutain).  
   N2MTG=1,                      !activation of ww diagnostics
   XTRIMF_TSURF_NIGHT=1.0,0.7,0.0,          !weight of surface temperature in each IFAC class   
   XLEVEL_DAY_START=1.33,2.0,5.0,           !start canopy level during day in each IFAC class   
   XLEVEL_DAY_END=2.0,2.0,5.0,              !end canopy level during day in each IFAC class   
   XLEVEL_NIGHT_START=1.0,1.0,5.0,          !start canopy level during night in each IFAC class   
   XLEVEL_NIGHT_END=1.0,1.0,5.0,            !end canopy level during night in each IFAC class     
   XLEVEL_K_DAY=1.0,                        !slope of linear function during day (1=till end of forecast)  
   XLEVEL_K_NIGHT=1.0,                      !slope of linear function during day (1=till end of forecast)  
   XMODIF_TLEV_START=0.0,0.0,0.0,           !heat wave joker in °C at init  
   XMODIF_TLEV_END=-2.0,-2.0,0.0,           !heat wave joker in °C at end of forecast  
   XMODIF_TSURF_START=0.0,0.0,0.0,          !tsurf joker in °C at init  
   XMODIF_TSURF_END=-2.0,-2.0,0.0,          !tsurf joker in °C at end of forecast  
   XMODIF_TLEV_K=7.,                        !at XMODIF_TLEV_THRES+XMODIV_TLEV_K the full heat wave joker will be applied  
   XMODIF_TLEV_THRES=308.,                  !threshold for heat wave joker in K  
   XSMOOTH_DAY_NIGHT=10800.,                !temporal smoothing for transition day/night in sec  

