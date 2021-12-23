.. _hpcstack:

==================================
HPC-Stack Platforms and Compilers
==================================

The HPC-Stack
=============

Definition 
-----------

HPC-stack is a repository that provides a unified, shell script-based
build system for building the software stack needed for the Unified
Forecasting System (UFS) and applications. HPC-stack is part of the
NCEPLIBS project and was written for the Joint Effort for Data
assimilation Integration (JEDI) framework.

How is the HPC-stack built?
---------------------------

To build the HPC-stack, all you would need is to have some prerequisites
installed and to follow three steps.

The prerequisites for building the HPC-stack are as follows: Lmod,
CMake, make, wget, curl, and git.

Building the HPC-stack

1. Configure the build

   a. In this step, choose the compiler, mpi, and python you would like
         to use, which is done by editing the config/config_custom.sh
         file.

   b. The next step is to choose the components of the stack, which is
         done by editing the stack/stack_custom.yaml file.

2. Setting up the compiler, MPI, Python, and Module System

   c. This step is only required if you are using LMod for managing the
         software stack. If LMod is required, run the setup_modules.sh
         script.

3. Build the Software Stack

   d. Lastly, build the stack using the build_stack.sh script. The
         configuration and yaml files used in Step 1 will be used here.

More details on how to build the stack can be found here: `GitHub -
NOAA-EMC/hpc-stack: Create a software stack for
HPC's <https://github.com/NOAA-EMC/hpc-stack>`__

HPC-Stack on NOAA HPC
=====================

An operational and a test HPC-stack have been installed on some NOAA
HPC. Both these stacks are updated a few times a month, with ESMF and
UPP being the packages that update more frequently than others.

The HPC-stack test versions are installed on Hera, Jet, Dell, WCOSS2,
and Orion. The locations of these HPC-stack versions can be found here:
`HPC stack test installations · NOAA-EMC/hpc-stack Wiki ·
GitHub <https://github.com/NOAA-EMC/hpc-stack/wiki/HPC-stack-test-installations>`__.

The official HPC-stack versions are installed on Hera, Orion,
WCOSS-Dell, Jet, Gaea, Cheyenne, WCOSS-2 (Acorn only). The locations of
these official HPC-stack can be found here: `Official Installations ·
NOAA-EMC/hpc-stack Wiki ·
GitHub <https://github.com/NOAA-EMC/hpc-stack/wiki/Official-Installations>`__.

Current Testing of HPC-Stack
============================

The HPC-stack has built-in GitHub Actions to verify if the HPC-stack
will build successfully using different platforms and compilers. These
GitHub actions are triggered every time there is a pull request. There
are four different yaml scripts that test these configurations by
building the HPC-stack and are found here: `hpc-stack/.github/workflows
at develop · NOAA-EMC/hpc-stack ·
GitHub <https://github.com/NOAA-EMC/hpc-stack/tree/develop/.github/workflows>`__.
An overview of the compiler, mpi, and python versions for the four yaml
scripts can be found in the table below. Note that the
test_download_only script only downloads the source code without
installing it.

+-------------+-------------+-------------+-------------+-------------+
| Script Name | Platform    | Compiler    | Mpi         | Python      |
|             | System      | (version)   | (version)   | version     |
+=============+=============+=============+=============+=============+
| build_intel | ubuntu-20.0 | Intel       | Impi        | 3.8         |
|             | 4           | (2021.4.0)  | (2021.4.0)  |             |
+-------------+-------------+-------------+-------------+-------------+
| build_macOS | Macos-10.15 | Clang       | Mpich       | 3.9.6       |
|             | (Catalina)  | (12.0.5)    | (3.3.2)     |             |
|             |             | (clang)     |             |             |
+-------------+-------------+-------------+-------------+-------------+
|             |             | Clang       |             |             |
|             |             | (12.0.5)    |             |             |
|             |             | (gcc)       |             |             |
+-------------+-------------+-------------+-------------+-------------+
| build_ubunt | ubuntu-20.0 | Gnu (9.3.0) | Openmpi     | 3.8         |
| u           | 4           |             | (4.1.1)     |             |
+-------------+-------------+-------------+-------------+-------------+
|             |             |             | Mpich       |             |
|             |             |             | (3.3.2)     |             |
+-------------+-------------+-------------+-------------+-------------+
| test_downlo | ubuntu-20.0 | Gnu (9.3.0) | Openmpi     | 3.9.4       |
| ad_only     | 4           |             | (4.0.1)     |             |
+-------------+-------------+-------------+-------------+-------------+

Both the build_intel and build_ubuntu use the stack_custom.yaml to build
the HPC-stack, while the build_macOS uses the stack_mac.yaml
configuration. The packages that are installed for both of these yaml
files can be found here:
`hpc-stack_yaml_pkgs <https://docs.google.com/spreadsheets/d/122bCY4Mh9LlR8ePTHOYfMI7xZWUtZaDbPH2krA-9Fn0/edit?usp=sharing>`__

The HPC-stack uses the Intel ‘classic’ compilers (ifort, icc, icpc),
which are equivalent to the new set of paid compilers. These Intel
‘classic’ compilers are still free and are a part of the Intel OneAPI
name.

New Platform and Compiler Testing of HPC-Stack
==============================================

Understanding how the HPC-stack is built on different platforms and
compilers is crucial to a repeatedly successful and consistent HPC-stack
build. The table below was created to document the commonly used and/or
new configurations of platforms, compilers, and library versions the
HPC-stack builds upon. This table contains a ‘Building Notes’ column
which will document how each configuration is built and allow for
comparison and contrast between different configurations.

+-----------+-----------+-----------+-----------+-----------+-----------+
| Script    | Platform  | Compiler  | Mpi       | Python    | Building  |
| Name      | System    | (version) | (version) | version   | Notes     |
+===========+===========+===========+===========+===========+===========+
|           |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+

NCEPLIBS Supported Platforms [Under construction]
=================================================

Tier 1 / (Support Level 1) Pre-configured

Cheyenne

Hera

Jet

Gaea

All of these systems already have NCEP Libs and NCEP-external pre-built,
which would suggest that the HPC Stack should run fine on these systems.

Tier 2 / Support Level 2 \| Configurable Platforms

Stampede2

Orion

This tier would require the NCEP libs and NCEP-external packages to be
built and once these packages are built, the UFS application should run
successfully. Given that, the HPC-stack should be able to be built on
these systems.

Tier 3 / Support Level3 \| Limited-Test Platforms

MacOS Mojave

MacOS Catalina

RedHat/CentOS Linux 8.1

Ubuntu Linux 18.04 LTS

The systems listed above have all been tested and can build and run the
MRW application. This would suggest that HPC Stack can be built on these
systems as well.

Required HPC-Stack packages for UFS WM
======================================

The HPC-stack is a collection of various packages used to address the 8
UFS Weather Model applications. As a result, there are some packages
listed in the stack yaml files that aren’t required to build the MRW or
SRW applications.

Both the MRW and SRW applications use the UFS WM at it’s core with the
UFS_UTILS repo. The required packages needed to build the MRW and SRW
applications can be found in the below table as well as the
`hpc-stack_yaml_pkgs <https://docs.google.com/spreadsheets/d/122bCY4Mh9LlR8ePTHOYfMI7xZWUtZaDbPH2krA-9Fn0/edit?usp=sharing>`__:

+-----------------------------------+-----------------------------------+
| UFS WM                            | UFS_UTILS                         |
+===================================+===================================+
| NetCDF (netcdf-fortran, netcdf-c, | NetCDF 4.3.3                      |
| HDF5, (+parallel), zlib)          |                                   |
+-----------------------------------+-----------------------------------+
| MPI                               | MPI                               |
+-----------------------------------+-----------------------------------+
| ESMF                              | ESMF 8.0.0                        |
+-----------------------------------+-----------------------------------+
| w3nco                             | W3nco 2.4.0                       |
+-----------------------------------+-----------------------------------+
| Bacio                             | Bacio 2.4.0                       |
+-----------------------------------+-----------------------------------+
| sp                                | Sp 2.3.3                          |
+-----------------------------------+-----------------------------------+
| FMS (R4|R8)                       | Sfcio 1.4.0                       |
+-----------------------------------+-----------------------------------+
| Python3                           | Nemsio 2.5.0                      |
+-----------------------------------+-----------------------------------+
| UPP (optional)                    | Sigio 2.3.0                       |
+-----------------------------------+-----------------------------------+
| PIO (optional)                    | Ip 3.3.3                          |
+-----------------------------------+-----------------------------------+
| OpenMP (optional)                 | G2 3.4.0                          |
+-----------------------------------+-----------------------------------+
|                                   | Wgrib2 2.0.8                      |
+-----------------------------------+-----------------------------------+
|                                   | Doxygen (optional)                |
+-----------------------------------+-----------------------------------+
|                                   | OpenMP (optional)                 |
+-----------------------------------+-----------------------------------+

Note for hpc-stack build on Orion: develop branch
=================================================

1. BUILD: NO or wget option of --no-check-certificate for madis pkg :
due to expired data certification

2. default miniconda3 installation option in stack_noaa.yaml: possible
conflict of conda environment setting with user’s local conda
configuration .condarc (restricted conda channels, etc.)
