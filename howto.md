--Instruction ------------------
singularity containers are standard way of running solid software. 
They are created by pull from docker containers on ifarm with the following command
module load singularity-2.6.0
cd /group/solid/apps/
setenv SINGULARITY_CACHEDIR ./
singularity pull docker://jeffersonlab/solid:[tag]

singularity containers are under /group/apps/ and the list are as of (2018/12/19):
jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s2.6.0.simg
jeffersonlab_solid_tag1.devel_s2.6.0.simg
jeffersonlab_solid_tag2.devel_s2.6.0.simg

On ifarm (or any jlab internal machine with nfs access to /group/apps/), you can load them directly
otherwise dowload them by "scp your_jlab_username@ftp.jlab.org:/group/solid/apps/[singularity_container_filename] ./"

singularity container naming convention:
for production container which won't changce once released
"orgnization_repository_tag_digest_uploadtime_singularityversion.singularityformat"
for devel container which would update from time to time
"orgnization_repository_tag_singularityversion.singularityformat"

The official github clone on ifarm for testing:
/group/solid/solid_github/JeffersonLab/solid_gemc
/group/solid/solid_github/gemc/source

*** load container *************************************************************
ssh -XY ifarm
(singularity will load your shell env,so clean them up to avoid conflict. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc"  temporally.)
module load singularity-2.6.0
cd your_work_dir  (which will the shared dir between host and container)
(the exmaple below is jeffersonlab/solid:1.0.0 on ifarm. Change to your path and your container)
(prepare for batch mode)
singularity shell -s /bin/tcsh /group/solid/apps/jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s2.6.0.simg
(prepare for graphic mode)
singularity run /group/solid/apps/jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s2.6.0.simg & 
vncviewer localhost:1
(exit container for batch mode)
control-d   (exit singularity)
(exit container for graphic mode)
control-c   (exit vncviewer)
fg          (bring singularity to front)
control-c   (exit singularity)

*** run jeffersonlab/solid:1.0.0 on ifarm *************************************************************
load jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s2.6.0.simg
(now you are in container, run following commands inside)
echo $SHELL      (check if you using tcsh, if not, run tcsh)
set prompt = '[#Container# %n@%m %c]$ '
(do this if you want to use the pre-installed solid_gemc on any machine)
source /solid/solid_gemc/set_solid 1.3
(run solid_gemc)
cd $SoLID_GEMC/script
solid_gemc solid_PVDIS_LD2_full.gcard
solid_gemc solid_SIDIS_He3_full.gcard

*** run jeffersonlab/solid:1.devel *************************
load jeffersonlab_solid_tag1.devel_s2.6.0.simg
(now you are in container, run following commands inside)
echo $SHELL      (check if you using tcsh, if not, run tcsh)
set prompt = '[#Container# %n@%m %c]$ '
(do this if you want to use the pre-installed solid_gemc)
source /solid/solid_gemc/set_solid
(do this if you want to test the official github solid_gemc on ifarm)
source /group/solid/solid_github/JeffersonLab/solid_gemc/set_solid 1.3 /jlab /group/solid/solid_github/JeffersonLab/solid_gemc
(do this if you want to test your github solid_gemc on any machine)
git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
source ${PWD}/solid_gemc/set_solid 1.3 /jlab ${PWD}/solid_gemc
cd $SoLID_GEMC/source/$GEMC_VERSION/
scons OPT=1 -j8
(run solid_gemc)
cd $SoLID_GEMC/script
solid_gemc solid_PVDIS_LD2_full.gcard
solid_gemc solid_SIDIS_He3_full.gcard

*** run jeffersonlab/solid:2.devel  ******************************************************************
load jeffersonlab_solid_tag2.devel_s2.6.0.simg
(now you are in container, run following commands inside)
echo $SHELL      (check if you using tcsh, if not, run tcsh)
set prompt = '[#Container# %n@%m %c]$ '
(do this if you want to use the pre-installed gemc and solid_gemc on any machine)
source /solid/solid_gemc/set_solid 2.3
setenv myGEMC /solid/source
(do this if you want to test official github gemc and github solid_gemc on ifarm)
source /group/solid/solid_github/JeffersonLab/solid_gemc/set_solid 2.3 /jlab /group/solid/solid_github/JeffersonLab/solid_gemc
setenv myGEMC /group/solid/solid_github/gemc/source
(do this if you want to test the official github gemc and your github solid_gemc on ifarm)
git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
source ${PWD}/solid_gemc/set_solid 2.3 /jlab ${PWD}/solid_gemc
setenv preGEMC ${GEMC}
setenv myGEMC /group/solid/solid_github/gemc/source
setenv GEMC ${myGEMC}
cd $SoLID_GEMC/source/devel
scons OPT=1 -j8
setenv GEMC ${preGEMC}
(do this if you want to test your github gemc and your github solid_gemc on any machine)
git clone https://github.com/gemc/source               (or just git pull to update it)
git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
source ${PWD}/solid_gemc/set_solid 2.3 /jlab ${PWD}/solid_gemc
setenv preGEMC ${GEMC}
setenv myGEMC ${PWD}/source
cd $myGEMC
scons OPT=1 LIBRARY=shared -j8
setenv GEMC ${myGEMC}
cd $SoLID_GEMC/source/devel
scons OPT=1 -j8
setenv GEMC ${preGEMC}
(get ready to run solid_gemc)
setenv LD_LIBRARY_PATH ${myGEMC}:${LD_LIBRARY_PATH}
setenv PATH /solid/banks/bin:${SoLID_GEMC}/source/devel:${PATH}
(run solid_gemc)
cd $SoLID_GEMC/script
solid_gemc solid_PVDIS_LD2_full.gcard
solid_gemc solid_SIDIS_He3_full.gcard
(run solid_gemc with new 3D field map)
cd $SoLID_GEMC/field/
wget https://hallaweb.jlab.org/12GeV/SoLID/download/field/solenoid_v1.dat
cd $SoLID_GEMC/script
solid_gemc solid_PVDIS_LD2_full_moved.gcard
solid_gemc solid_SIDIS_He3_full_moved.gcard
