MRW Container Workflow
----------------------

Introduction
------------

The Medium Range Weather (MRW) Application is one of the many
configurations of the Unified Forecasting System
(`UFS <https://ufs-weather-model.readthedocs.io/en/ufs-v2.0.0/>`__),
which is a community-based, coupled, comprehensive Earth modeling
system. The UFS is an open-sourced project made available to the public
and researchers, and is the future forecasting suite for NOAA’s
operational numerical weather prediction applications.

The UFS can be configured into `multiple
applications <https://ufscommunity.org/science/aboutapps/>`__, and this
document describes the MRW Application which targets predictions of
atmospheric behavior out to two weeks. The MRW Application v1.1 includes
a prognostic atmospheric model, pre- and post-processing tools, and a
community workflow. This release is designed to be one that the
community can run and improve on, and portable to a set of commonly used
platforms.

This document will expound on each piece of the MRW Application as well
as discuss the workflow for the containerized MRW Application. For
further reading, please check out the current MRW Application
`document <https://ufs-mrweather-app.readthedocs.io/en/ufs-v1.1.0/>`__
and `GitHub
repository <https://github.com/ufs-community/ufs-mrweather-app>`__.

Pre-processor Utilities and Initial Conditions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The UFS MRW Application utilizes the GFS analyses for initialization and
it can be in several formats such as the Gridded Binary v2
(`GRIB2 <https://ufs-mrweather-app.readthedocs.io/en/ufs-v1.1.0/glossary.html#term-GRIB2>`__)
format (in 0.50, or 1.0 degree grid spacing), or the NOAA Environmental
Modeling System
(`NEMS <https://ufs-mrweather-app.readthedocs.io/en/ufs-v1.1.0/glossary.html#term-NEMS>`__)
Input/Output
(`NEMSIO <https://ufs-mrweather-app.readthedocs.io/en/ufs-v1.1.0/glossary.html#term-NEMSIO>`__)
formats. The initial conditions may be pre-staged on disk by the user or
automatically downloaded by the workflow. The MRW Application is
distributed with the chgres_cube pre-processing software, and converts
the Global Forecast System (GFS) analyses to six tiles in Network Common
Data Form
(`NetCDF <https://ufs-mrweather-app.readthedocs.io/en/ufs-v1.1.0/glossary.html#term-NetCDF>`__)
format, which is used as input data for the model. More information
about chgres_cube can be found
`here <https://ufs-utils.readthedocs.io/en/ufs-v1.1.0/>`__.

Forecast Model
^^^^^^^^^^^^^^

The Finite-Volume Cubed-Sphere
(`FV3 <https://noaa-emc.github.io/FV3_Dycore_ufs-v2.0.0/html/index.html>`__)
dynamic core is the prognostic atmospheric model in the UFS MRW
Application. Currently, there are four supported model resolutions (13,
25, 50, and 100-km) for the MRW with 64 vertical levels.

The Common Community Physics Package
(`CCPP <https://dtcenter.org/community-code/common-community-physics-package-ccpp>`__)
are packages used by the model that describe small-scale physics like
clouds and radiations. There are two sets currently being worked on: the
Rapid Refresh Forecast System (RRFS) and the Global Forecast System
(GFS). There are two variants of the GFS CCPP the user can choose
(operational or experimental). The experimental suite (GFS v16)
initializes and evolves sea surface temperatures differently than the
current GFS CCPP suite (GFS v15). A scientific description of the CCPP
parameterizations and suites can be found
`here <https://dtcenter.ucar.edu/GMTB/v5.0.0/sci_doc/index.html>`__.

The UFS Weather Model ingests NetCDF files produced by chgres_cube and
outputs files in NetCDF format on a Gaussian grid in the horizontal and
model levels in the vertical.

Post-processor
^^^^^^^^^^^^^^

The Unified Post Processor is part of the workflow for the MRW
Application, and converts the NetCDF output to GRIB2 format. These newly
converted files can be used to visualize, plot, and verify the weather
model output. Learn more about UPP
`here <https://upp.readthedocs.io/en/upp-v9.0.0/>`__.

Visualization 
^^^^^^^^^^^^^^

At the time of this release, there’s no support for model visualization.
There are four basic NCAR Command Language (NCL) scripts to create basic
visualization of model output to do a quick visual check to verify that
the application is producing reasonable results.This scripts are
available at this `FTP
site <ftp://ftp.emc.ncep.noaa.gov/EIB/UFS/visualization_example/>`__.

Build System and Workflow
^^^^^^^^^^^^^^^^^^^^^^^^^

A CMake-based build system is used to build the required components for
running the MRW Application, which include the pre and post processing
software and UFS WM but not the
`NCEPLIBS-external <https://ufs-srweather-app.readthedocs.io/en/ufs-v1.0.1/Glossary.html#term-nceplibs-external>`__
and
`NCEPLIBS <https://ufs-srweather-app.readthedocs.io/en/ufs-v1.0.1/Glossary.html#term-nceplibs>`__
packages. Both NCEPLIBS packages are necessary to build the UFS WM and
are preinstalled on some `NOAA HPC
machines <https://github.com/ufs-community/ufs-srweather-app/wiki/Supported-Platforms-and-Compilers>`__.
In the case of the containerized MRW Application, the model is already
built.

The Common Infrastructure of Modeling the Earth
(`CIME <http://esmci.github.io/cime/versions/ufs_release_v1.0/html/index.html>`__)
Case Control System is used as the workflow manager for the MRW
Application. CIME is able to build the model and the workflow but cannot
build the NCEP libraries or run the pre and post processing tools. The
containerized MRW Application doesn’t use CIME as the workflow manager
for the model.

Containerized MRW Application
-----------------------------

Overview
^^^^^^^^

This containerized version of the MRW Application is intended for ease
of portability of application to on-prem or cloud platforms, ensuring
consistent and efficient builds, and faster development cycle. This
would also allow users to run test cases on the model quickly and
efficiently without worrying about the MRW Application failing.

A sample case was created for the containerized MRW Application and the
instructions to run said case can be found in the following sections. It
should be noted that refinement of the containerized MRW Application is
anticipated as this project is still considered a proof of concept.

Container Type
^^^^^^^^^^^^^^

Both `Docker <https://www.docker.com/resources/what-container>`__ and
`Singularity <https://sylabs.io/guides/3.5/user-guide/introduction.html>`__
containers were tested for this project, and both offer advantages and
disadvantages. Docker is well known throughout the software industry,
which leads to more user exposure. But Docker also has security
vulnerabilities if not used properly. Singularity is another container
software company that creates containers designed to run on High
Performance Computing (HPC) platforms with additional security measures.
Since the containerized MRW Application is expected to run on NOAA HPC
platforms, the Singularity container was chosen for this project.

Sample Case Workflow
^^^^^^^^^^^^^^^^^^^^

The containerized MRW Application sample case workflow consists of a
main workflow script and a regression test sample case. Minimal user
terminal interaction is required. However, users should have a good
understanding of basic Linux commands.

The MRW Application is built from the
`HPC-Stack <https://github.com/NOAA-EMC/hpc-stack>`__ and is a
simplified version of the MRW Application as it is configured for
atmosphere only, uses the inline post-processor and chgres_cube, and
doesn’t contain the CIME workflow. As a result, the current sample case
can be run on Ubuntu 20.04 OS or NOAA HPC platforms. The sample case
uses low resolution (c48) data and a forecasted period of three hours
making it easy to run the entire workflow within an hour and the
regression testing within two hours.

It should be noted again that the container itself and the accompanied
workflow are currently in a proof of concept state, so changes to the
container, workflow, and documentation are expected. Also, significant
work remains to ensure the containers that are offered to the community
are fully in sync with the UFS software used on the cloud and Tier-1
platforms.

Prerequisites
             

There are a couple of packages, data sets, and a script one must have
before running the containerized MRW Application. Below is a list of
these items:

*  Singularity installed locally

*  The MRW Application Singularity Image

*  The sample workflow case data: `c48.cold.tar.gz <https://drive.google.com/file/d/1GxM21loaQETcYRMEyqyHFRx7UscoRtrF/view>`__

*  Workflow script: `workflow.sh <https://drive.google.com/file/d/1dzRRkLha9M6Zq7augmdsgQywpjbVbfDz/view?usp=sharing>`__

*  The sample regression test case data: control_48.tar.gz

Workflow
        

*  Download the MRW Application Singularity Image by doing the following::

     singularity pull library://dcvelobrew/default/mrwufs-src:latest

*  Place the c48.cold.tar.gz file, workflow.sh, and mrwufs-src_latest.sif all in the same directory. An example would be to place everything here: /home/<user_name>/mrw_workflow/

*  Once the files and Singularity image are in the same directory, run this command::

     ./workflow.sh

*  Expected Results: this script will untar the c48 data and initiate the MRW Application to run a 3 hour forecast. It will also download a new GFS initial conditions dataset and run chgres_cube and create another 3 hour forecast. It can take anywhere from 10 mins to up to an hour to complete. A successful output should look like this::

      Tabulating mpp_clock statistics across      6 PEs...
      
                                            hits          tmin          tmax          tavg          tstd  tfrac grain pemin pemax
      Total runtime                            1   1810.551122   1810.551129   1810.551125      0.000003  1.000     0     0     5
      Initialization                           1      0.000000      0.000000      0.000000      0.000000  0.000     0     0     5
      FV dy-core                              96   1625.697105   1627.181261   1626.345525      0.481404  0.898    11     0     5
      FV subgrid_z                            48      2.780687      3.095576      2.957205      0.099777  0.002    11     0     5
      FV Diag                                 48      0.801515      1.031477      0.890374      0.077319  0.000    11     0     5
      GFS Step Setup                          96     12.575060     13.406329     13.111452      0.302518  0.007     1     0     5
      GFS Radiation                           48      5.054733      5.772690      5.431244      0.263562  0.003     1     0     5
      GFS Physics                             48      4.706983      5.128127      4.910280      0.139108  0.003     1     0     5
      Dynamics get state                      48      2.543151      2.849283      2.671299      0.108357  0.001     1     0     5
      Dynamics update state                   48     21.178364     22.646339     21.594512      0.558510  0.012     1     0     5
      FV3 Dycore                              96   1631.851340   1633.636120   1632.695959      0.564533  0.902     1     0     5
       MPP_STACK high water mark=           0
       wrt grid comp destroy time=  0.44805073299994547     


           ENDING DATE-TIME    DEC 20,2021  11:31:26.038  354  MON   2459569
           PROGRAM nems      HAS ENDED.
      * . * . * . * . * . * . * . * . * . * . * . * . * . * . * . * . * . * . * . * . 
      *****************RESOURCE STATISTICS*******************************
      The total amount of wall time                        = 1811.384090
      The total amount of time in user mode                = 23583.071477
      The total amount of time in sys mode                 = 4.532416
      The maximum resident set size (KB)                   = 567472
      Number of page faults without I/O activity           = 968688
      Number of page faults with I/O activity              = 0
      Number of times filesystem performed INPUT           = 31048
      Number of times filesystem performed OUTPUT          = 697680
      Number of Voluntary Context Switches                 = 511709
      Number of InVoluntary Context Switches               = 2447403
      *****************END OF RESOURCE STATISTICS*************************
         
Regression Testing
                  

The current `UFS regression test <https://ufs-weather-model.readthedocs.io/en/latest/BuildingAndRunning.html?highlight=rt#using-the-regression-test-script>`__ process (rt.sh) is designed to run a large number of UFS global and limited-area configurations in a full environment. But since the containerized MRW Application is limited in resources, most regression test cases will not work, with the exception of the c48 control case. Therefore, the lone control case was captured and saved as the control_48 tarfile.

Running the regression test c48 is similar to what was done previously.
See instructions below.

*  Place the control_48.tar.gz in the same directory as the mrwufs-src_latest.sif.

*  Untar the control_48 file using this command::

     tar xzvf control_48.tar.gz

*  cd into the control_48 folder and run the command below to start the regression test.::

     singularity exec -B $PWD:$PWD ../mrwufs-src_latest.sif mpirun -n 8 /home/builder/ufs-weather-model/ufs_model

*  A few things to note:

   * The model runs on 8 nodes instead of 6

   * The control_48 case can take up to two hours to complete.

.. |image0| image:: mrw-image.png
   :width: 6.5in
   :height: 4.88889in
