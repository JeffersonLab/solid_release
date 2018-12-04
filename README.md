# solid_release
release of all SoLID software packages

Instructions:  (replace the tag 1.0.0 with the version you want to run)
--------------------
To run in batch mode:

docker run -it --rm jeffersonlab/solid:1.0.0 bash

To use vnc via vncviewer or webbrowser:

docker run -it --rm -p 6080:6080 -p 5901:5901 jeffersonlab/solid:1.0.0

vncviewer localhost:1 or firefox localhost:6080

To build and push the container:

docker build -f Dockerfile-1.0.0 -t solid:1.0.0 .

docker tag solid:1.0.0 jeffersonlab/solid:1.0.0

sudo docker push jeffersonlab/solid:1.0.0

--------------------

For more detailed general instruction about singularity and docker, include jlab ifarm singularity installation and how to use them on windows and mac, refer to

https://hallaweb.jlab.org/wiki/index.php/Note_about_container


