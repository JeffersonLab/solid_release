instruction on ifarm/OSG for event generator container "solidevgen"

# run a pre-compiled generator
--------------------
* cd your_work_dir
* setenv location /scigroup/cvmfs (on ifarm)
* setenv location /cvmfs/oasis.opensciencegrid.org/jlab (on OSG)
* module load singularity/3.9.5
* run evgen_inclusive
  * singularity exec -B $location/halla/solid/soft/solidevgen_tag1:/evgen $location/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive/run commitaa2a325
* run evgen_inclusive_e
  * singularity exec -B $location/halla/solid/soft/solidevgen_tag1:/evgen $location/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive_e/run commitfea2654

# compile a generator for the official location
--------------------
* cd /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1 (change this for your personal location)
* setenv location /scigroup/cvmfs (on ifarm)
* module load singularity/3.9.5
* singularity shell -s /bin/tcsh -B ${PWD}:/evgen $location/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif
* set prompt = '[#Container# %n@%m %c]$ '
* compile evgen_inclusive
  * cd /evgen/evgen_inclusive
  * source setup 
  * git clone https://github.com/JeffersonLab/evgen_inclusive commitaa2a325
  * cd commitaa2a325
  * make
* compile evgen_inclusive_e
  * cd /evgen/evgen_inclusive_e
  * source setup
  * git clone https://github.com/JeffersonLab/evgen_inclusive_e commitfea2654
  * cd commitfea2654
  * cmake3 .
  * make
