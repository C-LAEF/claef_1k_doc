---
layout: default
title: Changes in C-LAEF 1k suite
nav_order: 8
---

### 23.4.2025 09 UTC
- Suite migrated to *zacs*

### 16.1.2025 09 UTC
- switch on lagged ensemble

### 10.12.2024 12 UTC
- major suite update
- enlarged domain
- fullpos revised

### 05.11.2024 09 UTC
- switch on 3DVAR for all members based on a new B-matrix
  
### 23.10.2024 09 UTC
- update Binaries, all cy46 binaries recompiled using ECMWF eccodes installation instead of Ryad's eccode installation on $PERM

### 22.10.2024 12 UTC
- all members on new domain  (Member 01 - Member17) initialized with ARPEGE surface, and EZ atmo
- surface assimilation switched on for Member 01-17

### 18.10.2024 00 UTC
- new setting to set-up first B-matrix
- member 00 as before, oper domain
- member 01-16 are running in downscaling mode (EZ atmo and soil) with +6h leadtime, large domain, slightly modified dynamic setup
- member 17, envar member on large domain, slightly modified dynamic setup
  
### 14.10.2024 06 UTC
- deactivate 2m obs at night in EnVar (-> https://github.com/C-LAEF/claef_1k/pull/76)

### 16.9.2024 06 UTC
- long runs for perturbed member switched off. All runs are only up to +6h for EnVar
- switch on member17: ctrl with EnVar, 00 UTC run up to +60h, all other runs up to +6h
- member 00: long runs for 00/06/12/18 UTC

### 13.08.2024 06 UTC
- member 00: ctrl. SLHD=T, COMAD=F + using fixes for radar (saturation, etc.) 
- member 01: ctrl. SLHD=T, COMAD=F
- member 02: ctrl. with dyn setup of MF (-> SLHD=F, COMAD=T) + using fixes for radar (saturation, etc.)
- member 03: ctrl. with SLHD=T, COMAD=T (for Q) + using fixes for radar (saturation, etc.)
- member 17: ctrl. with Envar data assimilation (not yet working, 001 segfault, switched of )
- all other members are similar to member 00 but with perturbations

### 02.08.2024 09 UTC
- member 00: ctrl. with dyn setup of MF + using fixes for radar (saturation, etc.) 
- member 01: ctrl. with dyn setup of MF (-> SLHD=F, COMAD=T)
- member 02: ctrl. with adapted dynamics setup (MF setup + REXPDH=2 adapted) + using fixes for radar (saturation, etc.)
- member 03: ctrl. with SLHD=T, COMAD=F + using fixes for radar (saturation, etc.)
- member 17: ctrl. with Envar data assimilation (not yet working, 001 segfault, switched of )
- all other members are perturbed members with dyn setup of MF

### 25.07.2024 09 UTC
- member 00: ctrl. with optimized dynamics setup (MF setup + RDAMPQ and RDAMPT adapted)
- member 01: ctrl. with dyn setup of MF (-> SLHD=F, COMAD=T)
- member 02: ctrl. with dyn setup of MF + using fixes for radar (saturation, etc.)
- member 03: ctrl. with SLHD=T (for GFL) and COMAD=T (for W and T)
- member 17: ctrl. with Envar data assimilation (not yet working, 001 segfault, switched of )
- all other members are perturbed members with dyn setup of MF
 
### 22.07.2024 09 UTC
- member 00: ctrl. with optimized dynamics setup (not yet decided, so MF setup used so far)
- member 01: ctrl. with dyn setup of MF (-> SLHD=F, COMAD=T)
- member 02: ctrl. with dyn setup of MF + using fixes for radar (saturation, etc.)
- member 17: ctrl. with Envar data assimilation (not yet working, 001 segfault, switched of from 12 UTC of 22.07.2024)
- all other members are perturbed members with dyn setup of MF

### 16.07.2024 12 UTC
- member 00 : dynamics setup changed (-> SLHD=T, COMAD=F=)
- member 01 : previous dyn setup (-> SLHD=F, COMAD=T .. MF setup)
- member 02:  using fixes for radar (saturation, etc.)

### 09.07.2024 12 UTC
- Transfer to ARSO implemented

### 08.07.2024 12 UTC
- Add updraft helicity tracks.
 
### 29.05.2024 09 UTC
- Restart full ensemble with 16+1 member
- inclusive 2m diagnostic of Wastl/Wittmann
- Including Radar reflectivities and GNSS
- Following forecast ranges: 00UTC -> +60h, 03 UTC -> +6h, 06 UTC -> +24h, 09/12/15/18/21UTC -> +6h
- Control Member with 4 long runs: 00/06/12/18 UTC -> +60h

### 14.05.2024 09 UTC
- adaptation of ww diagnostics based on verfication results. This only affects member 03.

### 02.05.2024 09 UTC
- New member setup and new binaries after accepting [arpifs_PR7](https://github.com/FlorianW-ZAMG/arpifs/pull/7) and [arpifs_PR6](https://github.com/FlorianW-ZAMG/arpifs/pull/6)
- New setup:  
  - Member 00: control run includes Radar Reflectivity
  - Member 01: same as Member 00 but with GNSS
  - Member 02: Old control setup without Radar
  - Member 03: Like Member 00 but with new 2m diagnostic from Wastl/Wittmann


Initial files from previous Member 01 (including Radar) were used for Member 00, 01, and 03. Intitial files from previous Member 00 were used for Member 02 to evaluate possible long term drying of Radar data.
Includes bug fix in namelist_lamflag3D

### 09.04.2024 06 UTC
- Add member 02 (linear trunc) and member 03 (quadratic trunc) to see impact of spectral truncation. Started in downscaling mode.

### 08.04.2024 06 UTC
- Adapted forecast ranges: 00UTC -> +60h, 06UTC -> +24h, 03/09/12UTC -> +6h, 15/18/21UTC -> +4h

### 25.03.2024 00 UTC
- Control member ensemble with 2 Member, Member_00 Control with passive Assimilation of GNSS, Member_01 use Radar reflectivity in 3D-Var. Member_02 has been switched off because of problems with stability (traj. underground).
  
### 21.03.2024 00 UTC
- Control member ensemble with 3 Member, Member_00 Control with passive Assimilation of GNSS, Member_01 use Radar reflectivity in 3D-Var, Member_02 with Latent Heat Nudging of INCA precipitation analysis

### 20.02.2024 06 UTC
- use revised Bmatrix 

### 12.02.2024 09 UTC
- Suite switched off, only CTRL continues to run

### 23.01.2024 09 UTC
- Esuite updated with latest developments in the repo including https://github.com/C-LAEF/claef_1k/pull/41 and https://github.com/C-LAEF/claef_1k/pull/42
- EnVAR Member switched off

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
