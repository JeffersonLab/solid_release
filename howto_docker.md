An example using Dockerfile-1.0.0 to build and run container jeffersonlab/solid:1.0.0 

To build and push the container:
* Create an empty directory and enter into the new directory and copy the needed Dockerfile into it
* docker build -f Dockerfile-1.0.0 -t solid:1.0.0 .
* docker tag solid:1.0.0 jeffersonlab/solid:1.0.0
* docker login                          (login with your docker account)
* docker push jeffersonlab/solid:1.0.0  (your docker account need the right privilege to work)
* docker rmi solid:1.0.0   (remove the tmp tag)

To use with docker
* docker pull jeffersonlab/solid:1.0.0 (pull latest version)
* docker run -it --rm jeffersonlab/solid:1.0.0 tcsh  (batch mode)
* docker run -it --rm -p 6080:6080 -p 5901:5901 jeffersonlab/solid:1.0.0  (graphic mode with vnc by vncviewer or webbrowser)
* vncviewer localhost:1 or firefox localhost:6080

