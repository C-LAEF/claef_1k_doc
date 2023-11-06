---
layout: default
title: Changes in C-LAEF 1k suite
nav_order: 1
---

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