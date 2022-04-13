# solid_release
Release of SoLID software packages by docker container and run by singularity

run [solid_gemc](https://github.com/JeffersonLab/solid_gemc)
--------------------
See details in [howto](https://github.com/JeffersonLab/solid_release/blob/master/howto.md)

(latest way) run solid_gemc outside of container [jeffersonlab/jlabce](https://hub.docker.com/r/jeffersonlab/jlabce/tags/)

(old way in 2018) run olid_gemc release v1.0.0 inside of container [jeffersonlab/solid:1.0.0](https://hub.docker.com/r/jeffersonlab/solid/tags/1.0.0)
which is based on container [jeffersonlab/jlabce:1.3m](https://hub.docker.com/r/jeffersonlab/jlabce/tags/1.3m) including jlab_version 1.3m with gemc 2.3m

run solid event generators 
--------------------
See details in [howto_evgen](https://github.com/JeffersonLab/solid_release/blob/master/howto_evgen.md)

build container 
--------------------
See details in [howto_docker](https://github.com/JeffersonLab/solid_release/blob/master/howto_docker.md)

Reference
--------------------
For more detailed general instruction about singularity and docker, include jlab ifarm singularity installation and how to use them on windows and mac, refer to https://hallaweb.jlab.org/wiki/index.php/Note_about_container

You may get vncviewer by "yum install tigervnc" or download a standalone version at https://www.realvnc.com/en/connect/download/viewer/linux/


