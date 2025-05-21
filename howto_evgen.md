here are instructions of running and compiling them using container at jlab ifarm (running on OSG will come later)

The top level location is /scigroup/cvmfs/halla/solid/soft, which is auto synced on OSG at /cvmfs/oasis.opensciencegrid.org/jlab/halla/solid/soft/

The container file is at "container/jeffersonlab_solidevgen_\*.sif"

The event generators built on the container are installed at "solidevgen_\*/\*"

some output are at /work/halla/solid/evgen/

jlab ifarm has been upgraded to Almalinux9 as of 2025

# run generators on ifarm
--------------------
* cd your_work_dir on ifarm
* setenv location /scigroup/cvmfs/halla/solid/soft
* setenv container solidevgen_tag1
* setenv evgenpath $location/$container
* (pick one paris of setting below)
  * setenv theevgen evgen_inclusive_e
  * setenv theversion commit0acacfe_20230908
  * setenv theevgen evgen_inclusive
  * setenv theversion commit517d0c6_20220527
  * setenv theevgen evgen_bggen
  * setenv theversion commite04ff27_20220405
* echo $evgenpath/$theevgen/$theversion (this is the final code dir)
* (run it with an existing input file as a test)
  * singularity exec -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre -B $location/:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/$theevgen/run $theversion
* (run it with your existing input file) 
  * cp $location/solidevgen_tag1/$theevgen/run ./ and modify it and copy input file
  * singularity exec -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre -B $location/:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif ./run $theversion
* use "shell -s /bin/tcsh" instead of "exec" to run them interactively
* more detailed examples running farm job, refer to https://github.com/JeffersonLab/solid_gemc/tree/master/script/farm

# compile generators on ifarm
--------------------
* setenv location /scigroup/cvmfs/halla/solid/soft
* setenv container solidevgen_tag1
* setenv evgenpath $location/$container
* cd $evgenpath (this is official location, change it for your personal location)
* singularity shell -s /bin/tcsh -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre -B ${PWD}:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif
* set prompt = '[#Container# %n@%m %c]$ '
* compile evgen_inclusive_e
  * cd /evgen/evgen_inclusive_e
  * source setup
  * git clone https://github.com/JeffersonLab/evgen_inclusive_e commit0acacfe_20230908 (check commit number and time on github first)
  * cd commit0acacfe_20230908
  * cmake3 .
  * make
* compile evgen_inclusive
  * cd /evgen/evgen_inclusive
  * source setup 
  * git clone https://github.com/JeffersonLab/evgen_inclusive commit517d0c6_20220527 (check commit number  and time on github first)
  * cd commit517d0c6_20220527
  * make
* compile evgen_bggen
  * cd /evgen/evgen_bggen
  * source setup
  * git clone https://github.com/JeffersonLab/evgen_bggen commite04ff27_20220405 (check commit number  and time on github first)
  * cd commite04ff27_20220405
  * make
