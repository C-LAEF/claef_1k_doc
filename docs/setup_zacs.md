---
layout: default
title: setup of user zacs
nav_order: 8
---

# Documentation of the setup of user zacs on ATOS

This page collects information about the setup of the time-critical user zacs on ATOS.

The user was setup to belong to Linux user group claef with access to SBU account claefop

### Installation of gmkpack

Currently only gmkpack.6.9.11 is installed under /home/zacs/tools

List of binaries in  double precision is defined in /home/zacs/tools/gmkpack_support/list/geosphere
Links for Snowgrid binaries in /home/zacs/tools/gmkpack_support/link

### Auxlibs

Currently only version 3.9 is installed under /home/zacs/packages


### Source code compilation
All packs are located in /ec/ws1/tc/zacs/home/packs

Before using gmkpack one has to source the gmkpack environment from /home/zacs/.env_gmkpack:
```
source /home/zacs/.env_gmkpack
```

available packs are:
- cy46_production.01.OMPIIFC2104SP.x: Single Precision binary, only MASTERODB
- cy46_production.01.OMPIIFC2104.x: Double Precision binaries