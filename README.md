# Docker_multistaging
Multistaging with Docker to reduce complexity, size, and build time

<img src="/Docs/multistage.jpg" width="600">  

[photo cred](https://www.youtube.com/watch?v=V9egJMknKtM)
&nbsp;  

### [1.  A brief intro on Docker and Image Size](#anchor1)
### [2.  Ways to reduce the size of a docker image](#anchor2)
### [3.  Docker multi-staging to the rescue (maybe)](#anchor3)
### [4.  A note on AWS and its lack of cache (by default)](#anchor4)
&nbsp;  
&nbsp;  
 

<a name="anchor1"></a>
# 1. A brief intro on Docker and Image Size 
<img src="/Docs/docker_small.png" width="400">  

[photo cred](https://learnk8s.io/blog/smaller-docker-images/) 

* A docker *image* is the "base" of the docker container. 
* A docker *image* is created by a *Dockerfile*, which is a set of instructions that are executed to make the docker *image*
&nbsp;  
&nbsp; 
### Image Size = Base Image + Essential Files + *Cruft* (a.k.a. random, unneeded files)
<img src="/Docs/cruft.jpg" width="400">  

[photo cred](https://slangit.com/meaning/cruft)  
  
### Reasons you should avoid large Docker images:
1. Increased time of image building, which can be detrimental to continuous integration and continuous deployment
2. 

<a name="anchor2"></a>
# 2. Ways to reduce the size of a docker image
<img src="/Docs/slim_docker.png" width="400">  

[photo cred](https://towardsdatascience.com/how-to-build-slim-docker-images-fast-ecc246d7f4a7/)  

* smaller base image
* fewer layers
* Use .dockerignore
* Add rm -rf /var/lib/apt/lists/* at the end of the apt-get -y (removes package manager cache)  
* Remove unnecessary dependencies with -–no-install-recommends flag
* Use multi-stage

# 3.  Docker multi-staging to the rescue (maybe)

Docker image layers are sort of like git commits, they store the difference between the previous and current version. 

However, layers use space, and the more layers that you have, the heavier your final image will be. Git repositories are similar,as the he size of your repo increases with the number of layers (Git has to store all the changes between commits).



