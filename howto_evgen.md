
The top level location on ifarm is /scigroup/cvmfs/halla/solid/soft, which is auto synced on OSG at /cvmfs/oasis.opensciencegrid.org/jlab/halla/solid/soft/

The container file is at "container/jeffersonlab_solidevgen_\*.sif"

The event generators built on the container are installed at "solidevgen_\*/\*"

here are instructions of running and compiling them on ifarm (running on OSG will come later)

# run generators on ifarm
--------------------
* cd your_work_dir
* setenv location /scigroup/cvmfs/halla/solid/soft (on ifarm)
* run evgen_inclusive
  * singularity exec -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre -B $location/solidevgen_tag1:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive/run commit517d0c6_20220527
* run evgen_inclusive_e
  * singularity exec -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre -B $location/solidevgen_tag1:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive_e/run commit0acacfe_20230908
* run evgen_bggen
  * singularity exec -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre -B $location/solidevgen_tag1:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_bggen/run commite04ff27_20220405
* more detailed examples running farm job, refer to https://github.com/JeffersonLab/solid_gemc/tree/master/script/farm
* use "shell -s /bin/tcsh" instead of "exec" to run them interactively
* copy and modify "$location/solidevgen_tag1/*/run" to customize your input files

# compile generators on ifarm9
--------------------
* cd /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1 (this is official location, change it for your personal location)
* setenv location /scigroup/cvmfs/halla/solid/soft
* singularity shell -s /bin/tcsh -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre -B ${PWD}:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif
* set prompt = '[#Container# %n@%m %c]$ '
* compile evgen_inclusive
  * cd /evgen/evgen_inclusive
  * source setup 
  * git clone https://github.com/JeffersonLab/evgen_inclusive commit517d0c6_20220527 (check commit number  and time on github first)
  * cd commit517d0c6_20220527
  * make
* compile evgen_inclusive_e
  * cd /evgen/evgen_inclusive_e
  * source setup
  * git clone https://github.com/JeffersonLab/evgen_inclusive_e commit6fc41a0_20220525 (check commit number and time on github first)
  * cd commit6fc41a0_20220525
  * cmake3 .
  * make
* compile evgen_bggen
  * cd /evgen/evgen_bggen
  * source setup
  * git clone https://github.com/JeffersonLab/evgen_bggen commite04ff27_20220405 (check commit number  and time on github first)
  * cd commite04ff27_20220405
  * make
