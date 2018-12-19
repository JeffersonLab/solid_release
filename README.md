# solid_release
Release of all SoLID software packages by docker container at [jeffersonlab/solid](https://hub.docker.com/r/jeffersonlab/solid/tags/) which are based on other container [jeffersonlab/jlabce](https://hub.docker.com/r/jeffersonlab/jlabce/tags/)

The production container won't change once release, but devel container will get updated from time to time

see analysis and got notification at
* https://anchore.io/image/dockerhub/jeffersonlab%2Fsolid%3A1.0.0
* https://anchore.io/preview/dockerhub/jeffersonlab%2Fsolid%3A1.devel
* https://anchore.io/preview/dockerhub/jeffersonlab%2Fsolid%3A2.devel

Instructions:  
--------------------
(replace the tag 1.0.0 with the version you want to run)

To use with singularity
* singularity pull docker://jeffersonlab/solid:1.0.0 (pull latest version)
* singularity shell -s /bin/tcsh solid-1.0.0.simg (batch mode)
* singularity run solid-1.0.0.simg (graphic mode with vnc by vncviewer or webbrowser)
* vncviewer localhost:1 or firefox localhost:6080

To use with docker
* docker pull jeffersonlab/solid:1.0.0 (pull latest version)
* docker run -it --rm jeffersonlab/solid:1.0.0 tcsh  (batch mode)
* docker run -it --rm -p 6080:6080 -p 5901:5901 jeffersonlab/solid:1.0.0  (graphic mode with vnc by vncviewer or webbrowser)
* vncviewer localhost:1 or firefox localhost:6080

To build and push the container:
* docker build -f Dockerfile-1.0.0 -t solid:1.0.0 .
* docker tag solid:1.0.0 jeffersonlab/solid:1.0.0
* docker login                          (login with your docker account)
* docker push jeffersonlab/solid:1.0.0  (your docker account need the right privilege to make this work)
* docker rmi solid:1.0.0   (remove the tmp tag)

See more details in [howto](https://github.com/JeffersonLab/solid_release/blob/master/howto)

Structure
--------------------
* jlabce is at /jlab
* solid softwares are at /solid

Reference
--------------------

For more detailed general instruction about singularity and docker, include jlab ifarm singularity installation and how to use them on windows and mac, refer to

https://hallaweb.jlab.org/wiki/index.php/Note_about_container


