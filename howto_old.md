this is outdated part from howto.md

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
