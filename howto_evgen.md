instruction on ifarm/OSG for event generator container "solidevgen"

# run a pre-compiled generator
--------------------
* cd your_work_dir
* setenv location /scigroup/cvmfs/halla/solid/soft (on ifarm)
* setenv location /cvmfs/oasis.opensciencegrid.org/jlab/halla/solid/soft (on OSG)
* module load singularity/3.9.5
* run evgen_inclusive
  * singularity exec -B $location/halla/solid/soft/solidevgen_tag1:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive/run commit517d0c6
* run evgen_inclusive_e
  * singularity exec -B $location/halla/solid/soft/solidevgen_tag1:/evgen $location/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive_e/run commit6fc41a0

# compile a generator for the official location
--------------------
* cd /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1 (change this for your personal location)
* setenv location /scigroup/cvmfs/halla/solid/soft (on ifarm)
* module load singularity/3.9.5
* singularity shell -s /bin/tcsh -B ${PWD}:/evgen $location/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif
* set prompt = '[#Container# %n@%m %c]$ '
* compile evgen_inclusive
  * cd /evgen/evgen_inclusive
  * source setup 
  * git clone https://github.com/JeffersonLab/evgen_inclusive commit517d0c6
  * cd commit517d0c6
  * make
* compile evgen_inclusive_e
  * cd /evgen/evgen_inclusive_e
  * source setup
  * git clone https://github.com/JeffersonLab/evgen_inclusive_e commit6fc41a0
  * cd commit6fc41a0
  * cmake3 .
  * make
* compile evgen_bggen
  * cd /evgen/evgen_bggen
  * source setup
  * git clone https://github.com/JeffersonLab/evgen_bggen commite04ff27
  * cd commite04ff27
  * make
