
Instructions  
--------------------

singularity container is the standard way of running solid software. 
They are created by pull from docker containers on ifarm with the following command
* module load singularity-2.6.1  (as of 2019/02/22)
* cd /group/solid/apps/
* setenv SINGULARITY_CACHEDIR ./
* singularity pull docker://jeffersonlab/solid:[tag]

singularity containers are under /group/solid/apps/ and the list are (as of 2019/02/22):
* jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s2.6.1.simg
* jeffersonlab_solid_tag1.devel_s2.6.1.simg
* jeffersonlab_solid_tag2.devel_s2.6.1.simg

On ifarm (or any jlab internal machine with nfs access to /group/apps/), you can load them directly.
otherwise dowload them by "scp your_jlab_username@ftp.jlab.org:/group/solid/apps/[singularity_container_filename] ./"

singularity container naming convention:
* for production container which won't change once released: "orgnization_repository_tag_digest_uploadtime_singularityversion.singularityformat"
* for devel container which would update from time to time: 
"orgnization_repository_tag_singularityversion.singularityformat"

The official github clone on ifarm for testing:
* /group/solid/solid_github/JeffersonLab/solid_gemc
* /group/solid/solid_github/gemc/source
* /group/solid/solid_github/maureeungaro/banks

# load container on ifarm
--------------------
* ssh -XY ifarm
(singularity will load your shell env,so clean them up to avoid conflict. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc"  temporally.)
* module load singularity-2.6.1
* cd your_work_dir  (which will the shared dir between host and container)
* (for batch mode)
  * singularity shell -s /bin/tcsh /group/solid/apps/jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s2.6.1.simg  (load for batch mode)
  * control-d   (exit singularity)
* (for graphic mode)
  * singularity run /group/solid/apps/jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s2.6.1.simg &   (load for graphic mode)
  * vncviewer localhost:5901  (access for for graphic mode, use the right port as shown in terminal)
  * control-c   (exit vncviewer)
  * fg          (bring singularity to front)
  * control-c   (exit singularity)

# run software after loading container 
--------------------

## run jeffersonlab/solid:1.0.0 
* load jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s2.6.1.simg
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
* load jeffersonlab_solid_tag1.devel_s2.6.1.simg
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
* load jeffersonlab_solid_tag2.devel_s2.6.0.simg
* (now you are in container, run following commands inside)
  * echo $SHELL      (check if you using tcsh, if not, run tcsh)
  * set prompt = '[#Container# %n@%m %c]$ '
* (do this if you want to compile and run the latest gemc,banks, and solid_gemc)
  * git clone https://github.com/gemc/source               (or just git pull to update it)
  * git clone https://github.com/maureeungaro/banks        (or just git pull to update it)
  * git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
  * setenv myGEMC /group/solid/solid_github/gemc/source    (this is officla copy on ifarm, you can change it to yours)
  * setenv myBANKS /group/solid/solid_github/maureeungaro/banks (this is officla copy on ifarm, you can change it to yours)
  * setenv mySoLID_GEMC /group/solid/solid_github/JeffersonLab/solid_gemc (this is officla copy on ifarm, you can change it to yours)
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
* (do this if you want to run the latest gemc,banks, and solid_gemc)
  * setenv myGEMC /group/solid/solid_github/gemc/source
  * setenv myBANKS /group/solid/solid_github/maureeungaro/banks  
  * setenv mySoLID_GEMC /group/solid/solid_github/JeffersonLab/solid_gemc
  * source $mySoLID_GEMC/set_solid 2.3 /jlab $mySoLID_GEMC 
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

