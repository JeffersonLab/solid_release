instruction on ifarm to test container "solidevgen"

(run a pre-compiled generator)
* module load singularity
* cd your_work_dir
* singularity exec -B /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1:/evgen /scigroup/cvmfs/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif /evgen/evgen_inclusive_e/run commitb5fb58e

(compile a generator)
* cd your_work_dir
* git clone https://github.com/JeffersonLab/evgen_inclusive_e commitb5fb58e
* singularity shell -s /bin/tcsh -B /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1:/evgen /scigroup/cvmfs/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_latest.sif
* set prompt = '[#Container# %n@%m %c]$ ' #now you are inside the container
* source /evgen/evgen_inclusive_e/setup
* cd commitb5fb58e
* cmake3 .
* make

