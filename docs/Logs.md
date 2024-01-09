---
layout: default
title: Changes in C-LAEF 1k suite
nav_order: 4
---

### 08.01.2024 00 UTC
- initialize member 17 with soil from member 00 and ECMWF atmo

### 02.01.2024 12 UTC
- initialize member 06 with first guess (soil+atmos) from member 00
- initialize member 17 with first guess (soil+atmos) from member 00 (including envar=0 for 12 UTC run)

### 13.12.2023 15 UTC
- remove snow in flat areas to reduce negative bias observed in some parts of Austria and Bavaria

### 05.12.2023 12 UTC
- major update of suite creation/scripts. Use config_oper.toml for esuite unter zat2. 
- In 12 UTC run, EDA an stochastic physics was switched on by mistake for all members. Fixed for 15 UTC run. 

### 22.11.2023 15 UTC
- switch off GPS assim. Has been active by mistake and was running without bias correction

### 16.11.2023 00 UTC
- Changed grib header of EnVar member 17 to control forecast

### 14.11.2023 12 UTC
- exchange all binaries with binaries from zat pack
  (/ec/ws1/tc/zat/tcwork/PACKS/)
- add snowgrid assimilation
- bugfix in gust calculation
  
### 9.11.2023 12 UTC
- exchange MASTERODB binary (ceilometer blacklisting)

### 8.11.2023 12 UTC
- change Lsoe in screening namelist

### 7.11.2023 15 UTC
- ceilometer added

### 7.11.2023 09 UTC
- New Bmatrix and reduced rednmc

### 6.11.2023 09, 12 UTC
- New binary with SPP bugfix, switch off CANOPY and update 2m diagnostics

### 31.10. 12 UTC - 3.11. 00 UTC
- New binary for SP with possible fix in SPP (found by Endi) was used, but resulted in crashes due to Trajectory underground/out of atm. So change was reverted

### 11.10.2023 09 UTC
- use sp-binary in 927 to make sure vertical levels are consistent
- reinitialize atmospere by downscaling

### 02.10.2023 06 UTC
- Downscaling to overcome hanging jobs that reoccured with the new month

### 26.9.2023 06 UTC
-  Major bug fix in EnVar. An even member number must be used for EnVar, so we switched to 48 Member (16+16+16). Also some bug fixes were implemented for EnVar

### 18.9.2023 06 UTC
-  Switch back to Wastl binary with fpos bugfix. After serious problems with the Suite (hanging jobs) around 11/12th of September the old SP-binary including the fpos bug was used

### 04.09.2023 9 UTC
-  Switch on EnVar Member. In total 51 member are used: 17 member from latest run + 17 member from latest 00 UTC run + 17 current ECMWF-ENS LBCs 

### 25.8. 09 UTC
-  New binary with fpos bug fix, that caused positive T-bias in lower atmosphere

### 17.7. 06 UTC
-  Switch to gridpoint fields in preparation for EnVar

### 12.7. 00 UTC
- Gribfile archiving on ECFS for the time beeing. Same fields as in C-LAEF MARS archive are stored

### 21.6.
- HARP extraction included
- screensurf,canari,pertsurf use np queue because we had long queuing times in nf. This can hopefully reverted after model upgrade at ECMWF on 27/6/2023
- usage of ECMWF LBCs from 48r1 esuite
- Archivinig of files for B-MAtrix implemented

### 9.6. 6 UTC
- Suite restarted with ARPEGE surface

### 8.6.
- Suite failed over public holiday, new initialisation with ECMWF surface for 00 UTC run

### 7.6.
- Fix usage of aviation and bufr temps in 3D-Var
- Update assimilation namelists

### 25.5. 0 UTC
- All members started with ARPEGE surface

### 24.5. 9 UTC
- Extension of suite to 16+1 member
- dirty sstex hack with python programm implemented
- mirror task implemented
- IO server activated

### 10.5. 6 UTC
- Use Single precision binary in 001. Runtime reduced from 490 seconds to 270 seconds for 3h forecast.
