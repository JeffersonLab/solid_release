instruction on ifarm to test event generator container "solidevgen"

# run a pre-compiled generator
--------------------
* module load singularity/3.9.5
* cd your_work_dir
* singularity exec -B /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1:/evgen /scigroup/cvmfs/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive_e/run commited4fe49

or on OSG, run the following
* singularity exec -B /cvmfs/oasis.opensciencegrid.org/jlab/halla/solid/soft/solidevgen_tag1:/evgen /cvmfs/oasis.opensciencegrid.org/jlab/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive_e/run commitfea2654

# compile a generator for the official location
--------------------
* cd /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1 (change this for your personal location)
* module load singularity/3.9.5
* singularity shell -s /bin/tcsh -B ${PWD}:/evgen /cvmfs/oasis.opensciencegrid.org/jlab/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif
* set prompt = '[#Container# %n@%m %c]$ '
* cd evgen_inclusive_e
* source /evgen/evgen_inclusive_e/setup
* git clone https://github.com/JeffersonLab/evgen_inclusive_e commitfea2654
* cd commitfea2654
* cmake3 .
* make

