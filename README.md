# solid_release
Release of all SoLID software packages by docker container at [jeffersonlab/solid](https://hub.docker.com/r/jeffersonlab/solid/tags/) which are based on other container [jeffersonlab/jlabce](https://hub.docker.com/r/jeffersonlab/jlabce/tags/)

The production container won't change once release, but devel container will get updated from time to time

see analysis and got notification at
* https://anchore.io/image/dockerhub/jeffersonlab%2Fsolid%3A1.0.0
* https://anchore.io/preview/dockerhub/jeffersonlab%2Fsolid%3A1.devel
* https://anchore.io/preview/dockerhub/jeffersonlab%2Fsolid%3A2.devel

# Quick Start

Following steps below to test solid software in graphic mode

* on jlab ifarm, prepare singularity by
 * "module load singularity"
 
* on your local centos/rhel/scientific linux, prepare singularity by "yum install singularity" and download https://hallaweb.jlab.org/12GeV/SoLID/download/singularity/jeffersonlab_jlabce_tagdevel_digestsha256:01eac4333bdd2077233076363983d1898775c6c61e8f5c5b0b9f324c75c4da3c_20200409_s3.5.3.sif

* load container by following 
 * "singularity run /group/solid/apps/jeffersonlab_solid_tag1.0.0_digestsha256:873524668b3b360392437188cf26f375cafc2e03a9bde6a58469a7dad8cc373a_20181204_s3.2.1.sif &" (load for graphic mode in background)
 * vncviewer localhost:5901 (access for graphic mode, use the right port as shown in terminal)

* run following inside vncviewer for solid simulation
  * echo $SHELL (check if you using tcsh, if not, run tcsh)
  * set prompt = '[#Container# %n@%m %c]$ '
  * source /solid/solid_gemc/set_solid 1.3
  * cd $SoLID_GEMC/script
  * solid_gemc solid_PVDIS_LD2_full.gcard
  * solid_gemc solid_SIDIS_He3_full.gcard

* Do the following to exit
 * close vncviewer
 * fg (bring singularity to front in the host)
 * control-c (exit singularity in the host)   

# Instructions:  
(replace the tag 1.0.0 with the version you want to run)

The official way to use it is with Singularity, see details in [howto](https://github.com/JeffersonLab/solid_release/blob/master/howto.md)

To use with docker
* docker pull jeffersonlab/solid:1.0.0 (pull latest version)
* docker run -it --rm jeffersonlab/solid:1.0.0 tcsh  (batch mode)
* docker run -it --rm -p 6080:6080 -p 5901:5901 jeffersonlab/solid:1.0.0  (graphic mode with vnc by vncviewer or webbrowser)
* vncviewer localhost:1 or firefox localhost:6080

To build and push the container:
* Create an empty directory and enter into the new directory and copy the needed Dockerfile into it
* docker build -f Dockerfile-1.0.0 -t solid:1.0.0 .
* docker tag solid:1.0.0 jeffersonlab/solid:1.0.0
* docker login                          (login with your docker account)
* docker push jeffersonlab/solid:1.0.0  (your docker account need the right privilege to make this work)
* docker rmi solid:1.0.0   (remove the tmp tag)

Structure
--------------------
* jlabce is at /jlab
* SoLID softwares are at /solid

Reference
--------------------

For more detailed general instruction about singularity and docker, include jlab ifarm singularity installation and how to use them on windows and mac, refer to

https://hallaweb.jlab.org/wiki/index.php/Note_about_container


