
# container list
--------------------
singularity container is the standard way of running solid software. 

(as of 2019/06/05)

on jlab machines, singularity container images are under /group/solid/apps/ and the list are:
* jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s3.2.1.sif (regular release and it won't change)
* jeffersonlab_solid_tag1.devel_s3.2.1.sif   (provide base package only and test latest other packages outside of contianer)
* jeffersonlab_solid_tag2.devel_s3.2.1.sif   (provide base package only and test latest other packages outside of contianer)

They are created by pull from docker container on jlab ifarm with the following command
* module load singularity
* cd /group/solid/apps/
* setenv SINGULARITY_CACHEDIR ./
* singularity pull docker://jeffersonlab/solid:[tag]

singularity container naming convention:
* for production container which won't change once released: "orgnization_repository_tag_digest_uploadtime_singularityversion.imageformat"
* for devel container which would update from time to time: 
"orgnization_repository_tag_singularityversion.imageformat"

# load container
--------------------
The example below is on jlab ifarm, farm, or any jlab internal machine with nfs access to /group/apps

On your local machine, you can dowload them by "scp your_jlab_username@ftp.jlab.org:/group/solid/apps/[singularity_container_filename] ./"

* ssh -XY ifarm
(singularity will load your shell env,so clean them up to avoid conflict. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc"  temporally.)
* module load singularity
* cd your_work_dir  (which will the shared dir between host and container)
* (for batch mode)
  * singularity shell -s /bin/tcsh /group/solid/apps/jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s3.2.1.sif  (load for batch mode)
  * control-d   (exit singularity)
* (for graphic mode)
  * singularity run /group/solid/apps/jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s3.2.1.sif &   (load for graphic mode)
  * vncviewer localhost:5901  (access for for graphic mode, use the right port as shown in terminal)
  * control-c   (exit vncviewer)
  * fg          (bring singularity to front)
  * control-c   (exit singularity)

# run software after loading container 
--------------------

## run jeffersonlab/solid:1.0.0 
* cd your working dir on host which will be bind into container automatically
* load jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s3.2.1.sif
* (now you are in container, run following commands inside)
  * echo $SHELL      (check if you using tcsh, if not, run tcsh)
  * set prompt = '[#Container# %n@%m %c]$ '
* (do this if you want to use the pre-installed solid_gemc on any machine)
  * source /solid/solid_gemc/set_solid 1.3
* (run solid_gemc)
  * cd $SoLID_GEMC/script
  * solid_gemc solid_PVDIS_LD2_full.gcard
  * solid_gemc solid_SIDIS_He3_full.gcard

## run jeffersonlab/solid:1.devel 
* cd your working dir on host which will be bind into container automatically
* load jeffersonlab_solid_tag1.devel_s3.2.1.sif
* (now you are in container, run following commands inside)
  * echo $SHELL      (check if you using tcsh, if not, run tcsh)
  * set prompt = '[#Container# %n@%m %c]$ '
* (do this if you want to use the pre-installed solid_gemc)
  * source /solid/solid_gemc/set_solid
* (do this if you want to test the official github solid_gemc on ifarm)
  * source /group/solid/solid_github/JeffersonLab/solid_gemc/set_solid 1.3 /jlab /group/solid/solid_github/JeffersonLab/solid_gemc
* (do this if you want to test your github solid_gemc on any machine)
  * git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
  * source ${PWD}/solid_gemc/set_solid 1.3 /jlab ${PWD}/solid_gemc
  * cd $SoLID_GEMC/source/$GEMC_VERSION/
  * scons OPT=1 -j8
* (run solid_gemc)
  * cd $SoLID_GEMC/script
  * solid_gemc solid_PVDIS_LD2_full.gcard
  * solid_gemc solid_SIDIS_He3_full.gcard

## run jeffersonlab/solid:2.devel  
* cd your working dir on host which will be bind into container automatically
* load jeffersonlab_solid_tag2.devel_s3.2.1.sif
* (now you are in container, run following commands inside)
  * echo $SHELL      (check if you using tcsh, if not, run tcsh)
  * set prompt = '[#Container# %n@%m %c]$ '
* (do this if you are at jlab machine and want to run the precompiled latest gemc,banks, and solid_gemc)
  * setenv myGEMC /group/solid/solid_github/gemc/source
  * setenv myBANKS /group/solid/solid_github/maureeungaro/banks  
  * setenv mySoLID_GEMC /group/solid/solid_github/JeffersonLab/solid_gemc
  * source $mySoLID_GEMC/set_solid 2.3 /jlab $mySoLID_GEMC 
  * setenv LD_LIBRARY_PATH ${myGEMC}:${LD_LIBRARY_PATH}
  * setenv PATH $myBANKS/bin:${SoLID_GEMC}/source/devel:${PATH}  
* (do this if you want to compile and run the latest gemc,banks, and solid_gemc yourself)
  * git clone https://github.com/gemc/source               (or just git pull to update it)
  * git clone https://github.com/maureeungaro/banks        (or just git pull to update it)
  * git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
  * setenv myGEMC $PWD/source    
  * setenv myBANKS $PWD/banks 
  * setenv mySoLID_GEMC $PWD/solid_gemc 
  * source $mySoLID_GEMC/set_solid 2.3 /jlab $mySoLID_GEMC
  * cd $myGEMC
  * scons OPT=1 LIBRARY=shared -j8
  * setenv preGEMC ${GEMC}
  * setenv GEMC ${myGEMC}
  * setenv preBANKS ${BANKS}
  * setenv BANKS ${myBANKS}
  * cd $myBANKS
  * scons OPT=1 -j8
  * cd $mySoLID_GEMC/source/devel
  * scons OPT=1 -j8
  * setenv GEMC ${preGEMC}
  * setenv LD_LIBRARY_PATH ${myGEMC}:${LD_LIBRARY_PATH}
  * setenv PATH $myBANKS/bin:${SoLID_GEMC}/source/devel:${PATH}
* (run solid_gemc)
  * cd $SoLID_GEMC/script
  * solid_gemc solid_PVDIS_LD2_full.gcard
  * solid_gemc solid_SIDIS_He3_full.gcard
* (run solid_gemc with new 3D field map)
  * cd $SoLID_GEMC/field/
  * wget https://hallaweb.jlab.org/12GeV/SoLID/download/field/solenoid_v1.dat
  * cd $SoLID_GEMC/script
  * solid_gemc solid_PVDIS_LD2_full_moved.gcard
  * solid_gemc solid_SIDIS_He3_full_moved.gcard


# run farm job with container 
--------------------
* examples at /work/halla/solid/sim/solid_gemc/SIDIS_He3_JLAB_VERSION_1.3/pass9/
 * run jeffersonlab/solid:1.0.0 with solid_gemc repo inside container "farm_solid_SIDIS_He3_BeamOnTarget_tag1.0.0_inside"
 * run jeffersonlab/solid:1.devel with solid_gemc repo inside container "farm_solid_SIDIS_He3_BeamOnTarget_tag1.devel_inside" and outside "farm_solid_SIDIS_He3_BeamOnTarget_tag1.devel_outside"
 * run jeffersonlab/solid:2.devel with solid_gemc repo inside container "farm_solid_SIDIS_He3_BeamOnTarget_tag2.devel_inside" and outside "farm_solid_SIDIS_He3_BeamOnTarget_tag2.devel_outside"

* Copy files (except any out* files) to your work dir to test it and works as follows
 * jobs run load_singularity.sh which load container to execute do_it_all.sh inside the container to run simulation, you can just run load_singularity.sh on ifarm to test a job locally
 * To submit jobs, you need to run LongRun_sim and give start run number and end run number, then it will call exec_sim creat jsub script and submit jobs.
 * The output will be created in the same dir
