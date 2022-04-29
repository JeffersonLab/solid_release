# quickstart
--------------------
here is a quickstart run run official solid_gemc software on jlab ifarm

* ssh -XY ifarm
* cd your working dir on host which will be bound into container automatically
* module load singularity/3.9.5
* singularity shell -s /bin/tcsh /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif
* (now you are in container, run following commands inside)
  * echo $SHELL      (check if you using tcsh, if not, run tcsh)
  * set prompt = '[#Container# %n@%m %c]$ ' (change shell promt to better tell where you are)
  * setenv SoLID_GEMC /group/solid/solid_github/JeffersonLab/solid_gemc
  * setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
  * setenv PATH ${SoLID_GEMC}/source/${GEMC_VERSION}:${PATH}
  * cd $SoLID_GEMC/script
  * solid_gemc solid_SIDIS_He3_moved_full.gcard
  * solid_gemc solid_PVDIS_LD2_moved_full.gcard

# container list
--------------------
singularity container is the standard way of running solid software. 

we run solid_gemc outside of container [jeffersonlab/jlabce](https://hub.docker.com/r/jeffersonlab/jlabce/tags/)

on jlab machines, singularity container images are under /group/solid/apps/ and the list are:
* latest
  * jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif (jlab_version 2.5 with gemc 2.9)
* outdated
  * jeffersonlab_jlabce_tag2.5_digest:sha256:431fa1b9d51d6225d05d179ba81dbf8ff77c222770966b1a9bfc2960f4dcd413_20211215_s3.8.3.sif (jlab_version 2.5 with gemc 2.9, but need ~/.jlab_software to work)
  * jeffersonlab_jlabce_tagdevel_digestsha256:01eac4333bdd2077233076363983d1898775c6c61e8f5c5b0b9f324c75c4da3c_20200409_s3.5.3.sif (jlab_version devel with gemc commit2fef2c2)
  * jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s3.2.1.sif (old way in 2018 to run olid_gemc release v1.0.0 inside of container [jeffersonlab/solid:1.0.0](https://hub.docker.com/r/jeffersonlab/solid/tags/1.0.0)
which is based on container [jeffersonlab/jlabce:1.3m](https://hub.docker.com/r/jeffersonlab/jlabce/tags/1.3m) including jlab_version 1.3m with gemc 2.3m)
  * jeffersonlab_solid_tag1.devel_s3.2.1.sif (like solid_tag1.0.0, but provide base package only and test other packages outside of contianer)
  * jeffersonlab_solid_tag2.devel_s3.2.1.sif (more test with base package only)

On your local machine, you can dowload container images by "scp your_jlab_username@ftp.jlab.org:/group/solid/apps/[singularity_container_filename] ./"

They are created by pulling from docker container on jlab ifarm with the following command
* module load singularity/3.9.5
* cd /group/solid/apps/
* setenv SINGULARITY_CACHEDIR /scratch/$USER
* setenv SINGULARITY_TMPDIR /scratch/$USER
* singularity pull docker://jeffersonlab/[name]:[tag]

singularity container naming convention:
* for production container which won't change once released: "organization_repository_tag_digest_uploadtime_singularityversion.imageformat"
* for devel container which would update from time to time: 
"orgnization_repository_tag_singularityversion.imageformat"

# load container
--------------------
The example below is on jlab ifarm, farm, or any jlab internal machine with direct access like nfs to /group/solid/apps/

* on jlab ifarm, prepare singularity by "module load singularity/3.9.5"
 
* on your local centos/rhel/scientific linux, prepare singularity by "yum install singularity"

To access any jlab internal machine or ifarm with graphic access by vnc, see https://hallaweb.jlab.org/wiki/index.php/Ifarm_graphic_mode

* ssh -XY ifarm
(singularity will load your shell env,so clean them up to avoid conflict. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc"  temporally.)
* module load singularity/3.9.5
* cd your_work_dir  (which will be shared dir between host and container automatically)
* (general terminal and graphic mode)
  * singularity shell -s /bin/tcsh /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif
  * run code inside container
  * control-d   
* (better graphic mode)
  * singularity run /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif &   (load for graphic mode in background)
  * vncviewer localhost:5901  (access for graphic mode, use the right port as shown in terminal)
  * run code inside container in vncviewer
  * close vncviewer
  * fg          (bring singularity to front in the host)
  * control-c   (exit singularity in the host)

# run software inside container 
--------------------

## run solid_gemc with jeffersonlab/jlabce:2.5

* cd your working dir on host which will be bound into container automatically
*  if you are on ifarm, run this
  * module load singularity/3.9.5
*  load container by
  * singularity shell -s /bin/tcsh /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif
* (now you are in container, run following commands inside)
  * echo $SHELL      (check if you using tcsh, if not, run tcsh)
  * set prompt = '[#Container# %n@%m %c]$ ' (change shell promt to better tell where you are)
* (do this if you are at jlab machine and want to run precompiled solid_gemc)
  * setenv SoLID_GEMC /group/solid/solid_github/JeffersonLab/solid_gemc
  * setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
  * setenv PATH ${SoLID_GEMC}/source/${GEMC_VERSION}:${PATH}
* (do this if you want to compile and run solid_gemc yourself)
  * git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
  * setenv SoLID_GEMC $PWD/solid_gemc
  * setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
  * setenv PATH ${SoLID_GEMC}/source/${GEMC_VERSION}:${PATH}
  * cd $SoLID_GEMC/field/
  * wget https://solid.jlab.org/files/field/solenoid_v4.dat (remember to change FIELD_DIR to ../field in gcard)
  * cd $SoLID_GEMC/source/${GEMC_VERSION}
  * make change to code
  * scons OPT=1 -j4
* (run solid_gemc with 3D field map) 
  * cd $SoLID_GEMC/script
  * solid_gemc solid_SIDIS_He3_moved_full.gcard
  * solid_gemc solid_PVDIS_LD2_moved_full.gcard

## run solid_gemc with jeffersonlab/jlabce:devel with gemc commit2fef2c2 (outdated)

* cd your working dir on host which will be bound into container automatically
*  if you are on ifarm, run this
  * module load singularity
*  load container by
  * singularity shell -s /bin/tcsh /group/solid/apps/jeffersonlab_jlabce_tagdevel_digestsha256:01eac4333bdd2077233076363983d1898775c6c61e8f5c5b0b9f324c75c4da3c_20200409_s3.5.3.sif
* (now you are in container, run following commands inside)
  * echo $SHELL      (check if you using tcsh, if not, run tcsh)
  * set prompt = '[#Container# %n@%m %c]$ ' (change shell promt to better tell where you are)
* (do this if you are at jlab machine and want to run precompiled solid_gemc)
  * setenv SoLID_GEMC /group/solid/solid_github/JeffersonLab/solid_gemc
  * setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
  * setenv PATH ${SoLID_GEMC}/source/commit2fef2c2:${PATH}
* (do this if you want to compile and run solid_gemc yourself)
  * git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
  * setenv SoLID_GEMC $PWD/solid_gemc
  * setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
  * setenv PATH ${SoLID_GEMC}/source/commit2fef2c2:${PATH} 
  * cd $SoLID_GEMC/field/
  * wget https://solid.jlab.org/files/field/solenoid_v2.dat  
  * cd $SoLID_GEMC/source/commit2fef2c2
  * make change to code
  * scons OPT=1 -j4
* (run solid_gemc with 3D field map) 
  * cd $SoLID_GEMC/script
  * solid_gemc solid_SIDIS_He3_moved_full.gcard
  * solid_gemc solid_PVDIS_LD2_moved_full.gcard

## run solid_gemc with jeffersonlab/solid:1.0.0  (outdated)
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

## run solid_gemc with jeffersonlab/solid:1.devel  (outdated)
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

## run solid_gemc with jeffersonlab/solid:2.devel  (outdated)
* cd your working dir on host which will be bound into container automatically
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
  * solid_gemc solid_SIDIS_He3_moved_full.gcard
  * solid_gemc solid_PVDIS_LD2_moved_full.gcard

# run farm jobs with container 
--------------------
see examples at https://github.com/JeffersonLab/solid_gemc/tree/master/script/farm

