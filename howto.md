# Introduction
--------------------
we run [solid_gemc](https://github.com/JeffersonLab/solid_gemc) using apptainer (formerly singularity) container. Currently we install solid_gemc outside the container  [jeffersonlab/jlabce](https://hub.docker.com/r/jeffersonlab/jlabce/tags/) which has gemc and geant4 inside

Because we running on the code in container, it doesn't really matter which machine to run it. but practically it still depends on existing files on disks. So henceforth, I **use "ifarm" to refer to jlab ifarm computers and any computer with access to /group/solid and use "standalone computer" to refer to any computer without the access**

The best way to run it is on jlab ifarm. 
you can run it in batch mode to produce output files with farm jobs or in graphic mode. 
If you are inside jlab network, you can run it on ifarm in graphic mode directly after "ssh -XY ifarm". 
Otherwise, the best way is to enter jlab network through [vnc](https://hallaweb.jlab.org/wiki/index.php/Ifarm_graphic_mode), and then run it on ifarm.

# run solid_gemc with graphic mode 
--------------------
According to your need, you can run it to look at existing simulation or modifiy to do develpment work as follows

here is a quick way to **run official solid_gemc installation on ifarm**
```
  (apptainer will load your shell env,so clean them up temporally to avoid conflict before loading container. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc")
 ssh -XY ifarm
 cd your_work_dir  (which will be shared dir between host and container)
 apptainer shell -s /bin/tcsh -B ${PWD}:/mywork -B /group,/u,/w,/cache,/volatile,/lustre /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif
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

To run your simulaton with modified geometry and without changing hit processing, here is how to **run official solid_gemc binary with your geometry on ifarm**
```
 (apptainer will load your shell env,so clean them up temporally to avoid conflict before loading container. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc")
 ssh -XY ifarm
 cd your_work_dir  (which will be shared dir between host and container)
 apptainer shell -s /bin/tcsh -B ${PWD}:/mywork -B /group,/u,/w,/cache,/volatile,/lustre /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif
 (now you are in container, run following commands inside)
   set prompt = '[#Container# %n@%m %c]$ '
   setenv SoLID_GEMC /group/solid/solid_github/JeffersonLab/solid_gemc
   setenv GEMC $SoLID_GEMC/mod/gemc/$GEMC_VERSION
   setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
   setenv PATH ${SoLID_GEMC}/source/${GEMC_VERSION}:${GEMC}:${PATH}
   cd /mywork
   git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
   (then you can modify simulation in your installation and run it as follows)
   cd solid_gemc/script
   solid_gemc solid_SIDIS_He3_moved_full.gcard
   solid_gemc solid_PVDIS_LD2_moved_full.gcard
   ctrl-d (to exit container)
```

To run your simulaton with modified geometry and changing hit processing, here is how to **run your solid_gemc binary with your installation on ifarm or a standalone computer with apptainer**
```
(apptainer will load your shell env,so clean them up temporally to avoid conflict before loading container. For example, "mv .cshrc cshrc", "mv .login login", "mv .bashrc basrc")
 cd your_work_dir  (which will be shared dir between host and container)
 (at ifarm or a standalone computer with /cvmfs using osg packages (https://osg-htc.org/docs/worker-node/install-cvmfs/) or cvmfsexec git (https://halldweb.jlab.org/wiki/index.php/HOWTO_use_prebuilt_GlueX_software_from_any_linux_user_account_using_cvmfsexec))
 apptainer shell -s /bin/tcsh -B ${PWD}:/mywork /cvmfs/oasis.opensciencegrid.org/jlab/halla/solid/soft/container/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif
 (at a standalone computer without cvmfs)
 wget http://webhome.phy.duke.edu/~zz81/simg/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif 
 apptainer shell -s /bin/tcsh -B ${PWD}:/mywork jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif
  (now you are in container, run following commands inside)
  set prompt = '[#Container# %n@%m %c]$ ' (change shell promt to better tell where you are)
  echo $SHELL      (check if you using tcsh, if not, you may need run tcsh)
  cd /mywork
  (then you can compile and solid_gemc)
  git clone https://github.com/JeffersonLab/solid_gemc   (or just git pull to update it)
  setenv SoLID_GEMC $PWD/solid_gemc
  setenv GEMC $SoLID_GEMC/mod/gemc/$GEMC_VERSION
  cd $GEMC
  scons OPT=1 LIBRARY=shared -j4
  setenv LD_LIBRARY_PATH ${GEMC}:${LD_LIBRARY_PATH}
  cd $SoLID_GEMC/source/${GEMC_VERSION}
  (make change to hit process codes if needed)
  scons OPT=1 -j4
  setenv PATH ${SoLID_GEMC}/source/${GEMC_VERSION}:${GEMC}:${PATH}
  cd $SoLID_GEMC/script
  (running solid_gemc in graphic mode as follows)
  solid_gemc solid_SIDIS_He3_moved_full.gcard
  solid_gemc solid_PVDIS_LD2_moved_full.gcard
  ctrl-d (to exit container)
```

Optional steps to use latest 3D field map
```
By default, SoLID use a small 2D field map to speed up program loading at interactive mode. To use a large 3D field map for real studies, do the following
  (at ifarm)
  modify the gcard as follows
  <option name="FIELD_DIR" value="/group/solid/www/solid/html/files/field"/>
  <option name="HALL_FIELD" value="solenoid_v4"/>
  <option name="FIELD_PROPERTIES" value="solenoid_v4, 1*mm, G4ClassicalRK4,linear"/>
  (at a standalone machine)
  cd  $SoLID_GEMC/field
  wget https://solid.jlab.org/files/field/solenoid_v4.dat
  modify the gcard as follows
  <option name="FIELD_DIR" value="../field"/>
  <option name="HALL_FIELD" value="solenoid_v4"/>
  <option name="FIELD_PROPERTIES" value="solenoid_v4, 1*mm, G4ClassicalRK4,linear"/>
```

# run solid_gemc with batch mode and farm jobs
--------------------
see examples at https://github.com/JeffersonLab/solid_gemc/tree/master/script/farm

# more information about using container 
--------------------

You need apptainer (better than docker) to run the container. on your computer, you need to install it 
* on local centos/rhel/scientific linux, prepare apptainer by "sudo yum install apptainer". on Ubuntu, use apt.
* on windows or Mac, install it by https://apptainer.org/docs/admin/main/installation.html and the last way is to use apptainer in a linux virtual machine
* refer to https://hallaweb.jlab.org/wiki/index.php/Note_about_container

If your linux machine is a desktop at jlab, you can ask computer center to help you mount /group/solid/ or /cvmfs, or you can use sshfs to mount them from jlabl1, then you can just use it like ifarm

The native graphic mode is inside container, you can use vnc to access as follows
  * apptainer run /group/solid/apps/jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif &   (load for graphic mode in background)
  * vncviewer localhost:5901  (access for graphic mode, use the right port as shown in terminal)
  * run code inside container in vncviewer
  * close vncviewer
  * fg          (bring apptainer to front in the host)
  * control-c   (exit apptainer in the host)

# container list
--------------------

on jlab computers, apptainer container images are under /group/solid/apps and they can be downloaded by "scp your_jlab_username@ftp.jlab.org:/group/solid/apps/[apptainer_container_filename] ./". The list includes:
* latest
  * jeffersonlab_jlabce_tag2.5_digest:sha256:9b9a9ec8c793035d5bfe6651150b54ac298f5ad17dca490a8039c530d0302008_20220413_s3.9.5.sif (jlab_version 2.5 with gemc 2.9)
* outdated
  * jeffersonlab_jlabce_tag2.5_digest:sha256:431fa1b9d51d6225d05d179ba81dbf8ff77c222770966b1a9bfc2960f4dcd413_20211215_s3.8.3.sif (jlab_version 2.5 with gemc 2.9, but need ~/.jlab_software to work)
  * jeffersonlab_jlabce_tagdevel_digestsha256:01eac4333bdd2077233076363983d1898775c6c61e8f5c5b0b9f324c75c4da3c_20200409_s3.5.3.sif (jlab_version devel with gemc commit2fef2c2)
  * jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s3.2.1.sif (old way in 2018 to run olid_gemc release v1.0.0 inside of container [jeffersonlab/solid:1.0.0](https://hub.docker.com/layers/jeffersonlab/solid/1.0.0)
which is based on container [jeffersonlab/jlabce:1.3m](https://hub.docker.com/layers/jeffersonlab/jlabce/1.3m) including jlab_version 1.3m with gemc 2.3m)
  * jeffersonlab_solid_tag1.devel_s3.2.1.sif (like solid_tag1.0.0, but provide base package only and test other packages outside of contianer)
  * jeffersonlab_solid_tag2.devel_s3.2.1.sif (more test with base package only)


apptainer container naming convention:
* for production container which won't change once released: "organization_repository_tag_digest_uploadtime_apptainerversion.imageformat"
* for devel container which would update from time to time: 
"orgnization_repository_tag_apptainerversion.imageformat"

They are created by pulling from docker container on jlab ifarm with the following command
* cd /group/solid/apps/
* setenv apptainer_CACHEDIR /scratch/$USER
* setenv apptainer_TMPDIR /scratch/$USER
* apptainer pull docker://jeffersonlab/[name]:[tag]
* apptainer pull docker://jeffersonlab/jlabce:2.5 (for exmaple, https://hub.docker.com/layers/jeffersonlab/jlabce/2.5)
