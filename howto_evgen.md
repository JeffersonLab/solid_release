instruction on ifarm to test container "solidevgen"

(run a pre-compiled generator)
* module load singularity
* cd your_work_dir
* singularity exec -B /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1:/evgen /scigroup/cvmfs/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_digeste414bfa974fb72484fdede70cc180527aec1daa913bfe80a9fc8d00910108f52_c20210412_s3.7.1.sif /evgen/evgen_inclusive_e/run

(compile a generator)
* cd your_work_dir
* git clone https://github.com/JeffersonLab/evgen_inclusive_e commitb5fb58e
* singularity shell -s /bin/tcsh -B /scigroup/cvmfs/halla/solid/soft/solidevgen_tag1:/evgen /scigroup/cvmfs/halla/solid/soft/container/jeffersonlab_solidevgen_tag1_digeste414bfa974fb72484fdede70cc180527aec1daa913bfe80a9fc8d00910108f52_c20210412_s3.7.1.sif
* set prompt = '[#Container# %n@%m %c]$ ' #now you are inside the container
* source /evgen/evgen_inclusive_e/setup
* cd commitb5fb58e
* cmake3 .
* make

