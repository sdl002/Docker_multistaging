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

### * A docker *image* is the "base" of the docker container. 
### * A docker *image* is created by a *Dockerfile*, which is a set of instructions that act as a multi-layered filesystem in Docker.
### * When docker runs the *image* in produce one (or many) containers.
&nbsp;  
&nbsp; 
### Image Size = Base Image + Essential Files + *Cruft* (a.k.a. random, unneeded files)
<img src="/Docs/cruft.jpg" width="400">  

[photo cred](https://slangit.com/meaning/cruft)  
  
### Reasons you should avoid large Docker images:
1. First and foremost: it’s best practices to maintain a small image size [Docker Docs](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
2. Large images increase image build time, including pushing/pulling, which can be detrimental to continuous integration and continuous development/deployment
3. Reducing unnecesary dependencies will decrease both the complexity and the chances of vulnerability in your application
4. 



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

However, layers use space, and the more layers that you have, the heavier your final image will be. Git repositories are similar, as the size of your repo increases with the number of layers (Git has to store all the changes between commits).

Keep this in mind



# 4. A note on AWS and its lack of cache (by default)

By default, AWS does not utilize cache. There are ways to turn it on (ask Josh :) ), but I have found that with a large build using multistaging can be useful.
