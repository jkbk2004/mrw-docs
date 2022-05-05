Using UFS Medium-Range Weather Application with Global Workflow
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Set-up
^^^^^^

1. Clone repository and checkout global workflow::

      git clone https://github.com/ufs-community/ufs-mrweather-app.git
      cd ufs-mrweather-app
      ./manage_externals/checkout_externals

2. Check if checkout of externals was successful using::

      ./manage_externals/checkout_externals -S

3. Build UFS model and global-workflow components::

      sh build_global-workflow.sh [-c]
      (ONLY use the -c option to compile for coupled UFS; requires different physics packages and APP argument when running
      setup_expt.py in step 4. )

4. Create a COMROT and EXPDIR. The experiment and workflow set-up scripts in following steps will point to these paths. Initial conditions will also need to be placed in COMROT.

5. Run experiment generator script::

      cd ufs-mrweather-app/global-workflow/ush/rocoto
      ./setup_expt.py forecast-only --pslot $EXP_NAME --idate
      2020010100 --edate 2020010118 --resdet 384 --gfs_cyc 4 --comrot
      $PATH_TO_YOUR_COMROT_DIR --expdir $PATH_TO_YOUR_EXPDIR

(example with COMROT and EXPDIR paths)::

      ./setup_expt.py forecast-only --pslot test --idate 2020010100 --edate 2020010118 --resdet 384 --gfs_cyc 4 --comrot /work/noaa/marine/Cameron.Book/ufs/COMROT --expdir /work/noaa/marine/Cameron.Book/ufs/EXPDIR




