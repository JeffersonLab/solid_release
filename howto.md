# Introduction
--------------------
we run [solid_gemc](https://github.com/JeffersonLab/solid_gemc) using container and singularity/apptainer. Currently we install solid_gemc outside the container  [jeffersonlab/jlabce](https://hub.docker.com/r/jeffersonlab/jlabce/tags/) which has gemc and geant4 inside

The best way to run it is on jlab ifarm machines. you can run it in batch mode to produce files and submit farm jobs or with graphic mode (refer to  https://hallaweb.jlab.org/wiki/index.php/Ifarm_graphic_mode)

# run solid_gemc with graphic mode 
--------------------
on ifarm or a jlab machine with access to /group/solid, you can run the official solid_gemc installation without modification or run your customized installation to do develpment work

here is a quick way to run official solid_gemc installation on ifarm
```
 (singularity will load your shell env,so clean them up temporally to avoid conflict before loading container. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc")
 ssh -XY ifarm9
 singularity shell -s /bin/tcsh -B ${PWD}:/mywork -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif
 (now you are in container, run following commands inside)
   set prompt = '[#Container# %n@%m %c]$ '
   setenv SoLID_GEMC /group/solid/solid_github/JeffersonLab/solid_gemc
   setenv GEMC $SoLID_GEMC/mod/gemc/$GEMC_VERSION
   setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
   setenv PATH ${SoLID_GEMC}/source/${GEMC_VERSION}:${GEMC}:${PATH}
   cd $SoLID_GEMC/script
   solid_gemc solid_SIDIS_He3_moved_full.gcard
   solid_gemc solid_PVDIS_LD2_moved_full.gcard
   ctrl-d (to exit container)
```

here are more details about how to run your customized installation on any machine
(The preferred way is to do this on ifarm, then you can use the container at /group/solid/apps and field file at /group/solid/www/solid/html/files/field without dowdnloading them. But it works on any machine after downloading)

```  
  (singularity will load your shell env,so clean them up temporally to avoid conflict before loading container. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc")
  cd your_work_dir  (which will be shared dir between host and container)
  wget http://webhome.phy.duke.edu/~zz81/simg/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif (on ifarm, no need to download the container)  
  singularity shell -s /bin/tcsh -B ${PWD}:/mywork jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif  (on ifarm, add option " -B /group:/group -B /u:/u -B /w/work:/work -B /w:/w -B /cache:/cache -B /volatile:/volatile -B /lustre:/lustre")
  (now you are in container, run following commands inside)
  echo $SHELL      (check if you using tcsh, if not, run tcsh)
  set prompt = '[#Container# %n@%m %c]$ ' (change shell promt to better tell where you are)
  cd /mywork
  (then you can compile and solid_gemc)
  git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
  setenv SoLID_GEMC $PWD/solid_gemc
  setenv GEMC $SoLID_GEMC/mod/gemc/$GEMC_VERSION
  cd $GEMC
  scons OPT=1 LIBRARY=shared -j4
  setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
  cd $SoLID_GEMC/source/${GEMC_VERSION}
  (make change to code if needed)
  scons OPT=1 -j4
  setenv PATH ${SoLID_GEMC}/source/${GEMC_VERSION}:${GEMC}:${PATH}
  cd  $SoLID_GEMC/field
  wget https://solid.jlab.org/files/field/solenoid_v4.dat (on ifarm, no need to download the field map)
  cd $SoLID_GEMC/script
  (if not on ifarm, modify gcard files to change FIELD_DIR to /mywork/solid_gemc/field before running solid_gemc in graphic mode as follows)
  solid_gemc solid_SIDIS_He3_moved_full.gcard
  solid_gemc solid_PVDIS_LD2_moved_full.gcard
  ctrl-d (to exit container)
```

# run solid_gemc with batch mode and farm jobs
--------------------
see examples at https://github.com/JeffersonLab/solid_gemc/tree/master/script/farm

# more information about using container 
--------------------

You need singularity (preferred) or docker to run the container. on your local machine, you need to install it 
* on local centos/rhel/scientific linux, prepare singularity by "yum install apptainer" (old system "yum install singularity"). on Ubuntu, use apt. If your linux machine is a desktop at jlab, you can self-mount or ask computer center to help you to mount /group/solid/, then you can just use it like ifarm
* on windows or Mac, try apptainer first https://apptainer.org/docs/admin/main/installation.html. If that somehow doesn't work, try docker https://docs.docker.com/desktop/install/mac-install/. The last way is to use singularity/docker in a virtual machine.
* refer to https://hallaweb.jlab.org/wiki/index.php/Note_about_container

The native graphic mode is inside container, you can use vnc to access as follows
  * singularity run /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif &   (load for graphic mode in background)
  * vncviewer localhost:5901  (access for graphic mode, use the right port as shown in terminal)
  * run code inside container in vncviewer
  * close vncviewer
  * fg          (bring singularity to front in the host)
  * control-c   (exit singularity in the host)

# container list
--------------------

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
They can be downloaded by "scp your_jlab_username@ftp.jlab.org:/group/solid/apps/[singularity_container_filename] ./"

singularity container naming convention:
* for production container which won't change once released: "organization_repository_tag_digest_uploadtime_singularityversion.imageformat"
* for devel container which would update from time to time: 
"orgnization_repository_tag_singularityversion.imageformat"

They are created by pulling from docker container on jlab ifarm with the following command
* module load singularity/3.9.5
* cd /group/solid/apps/
* setenv SINGULARITY_CACHEDIR /scratch/$USER
* setenv SINGULARITY_TMPDIR /scratch/$USER
* singularity pull docker://jeffersonlab/[name]:[tag]
