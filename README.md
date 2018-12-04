# solid_release
release of all SoLID software packages

They are put into docker containers at [jeffersonlab/solid](https://hub.docker.com/r/jeffersonlab/solid/tags/) which is based on [jeffersonlab/jlabce](https://hub.docker.com/r/jeffersonlab/jlabce/tags/)

Instructions:  
--------------------
(replace the tag 1.0.0 with the version you want to run)

To use with singularity
* singularity pull docker://jeffersonlab/solid:1.0.0
* singularity shell -s /bin/tcsh solid-1.0.0.simg

To use with docker
* docker run -it --rm jeffersonlab/solid:1.0.0 tcsh  (batch mode)
* docker run -it --rm -p 6080:6080 -p 5901:5901 jeffersonlab/solid:1.0.0  (graphic mode with vnc by vncviewer or webbrowser)
* vncviewer localhost:1 or firefox localhost:6080

To build and push the container:
* docker build -f Dockerfile-1.0.0 -t solid:1.0.0 .
* docker tag solid:1.0.0 jeffersonlab/solid:1.0.0
* sudo docker push jeffersonlab/solid:1.0.0  (your docker account would need the right privilege)

--------------------

For more detailed general instruction about singularity and docker, include jlab ifarm singularity installation and how to use them on windows and mac, refer to

https://hallaweb.jlab.org/wiki/index.php/Note_about_container


