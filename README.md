# Docker_multistaging
Multistaging with Docker to reduce complexity, size, and build time

<img src="/Docs/multistage.jpg" width="600">  

&nbsp;  

### [1.  A brief intro on Docker and Image Size](#anchor1)
### [2.  Ways to reduce the size of a docker image](#anchor2)
### [3.  AWS and its lack of cache (by default)](#anchor3)
### [4.  Docker multi-staging to the rescue (maybe)](#anchor4)

&nbsp;  
&nbsp;  
 

<a name="anchor1"></a>
# 1. A brief intro on Docker and Image Size 
<img src="/Docs/docker_small.png" width="400">  

* A docker *image* is the "base" of the docker container. 
* A docker *image* is created by a *Dockerfile*, which is a set of instructions that are executed to make the docker *image*

### Image Size = Base Image + Essential Files + *Cruft* (a.k.a. random, unneeded files)


<a name="anchor2"></a>
# 2. Ways to reduce the size of a docker image
<img src="/Docs/slim_docker.png" width="400">  
[photo cred](https://towardsdatascience.com/how-to-build-slim-docker-images-fast-ecc246d7f4a7)  

* smaller base image
* fewer layers
* Use .dockerignore
* Add rm -rf /var/lib/apt/lists/* at the end of the apt-get -y install to clean up after installing packages

