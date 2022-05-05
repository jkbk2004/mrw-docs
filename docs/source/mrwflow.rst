Using UFS Medium-Range Weather Application with Global Workflow
---------------------------------------------------------------

Set-up

*  Clone repository and checkout global workflow::
      git clone https://github.com/ufs-community/ufs-mrweather-app.git
      cd ufs-mrweather-app
      ./manage_externals/checkout_externals

* Check if checkout of externals was successful using::
      ./manage_externals/checkout_externals -S

* Build UFS model and global-workflow components::
      sh build_global-workflow.sh [-c]
      (ONLY use the -c option to compile for coupled UFS; requires
      different physics packages and APP argument when running
      setup_expt.py in step 4. )

*  A few things to note:

   * The model runs on 8 nodes instead of 6

   * The control_48 case can take up to two hours to complete.


