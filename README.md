# Docker_multistaging
Multistaging with Docker to reduce complexity, size, and build time

<img src="/Docs/multistage.jpg" width="600">  

[photo cred](https://www.youtube.com/watch?v=V9egJMknKtM)
&nbsp;  

### [1.  A brief intro on Docker and Image Size](#anchor1)
### [2.  Ways to reduce the size of a docker image](#anchor2)
### [3.  Docker multi-staging to the rescue](#anchor3)
### [4.  A note on AWS and its lack of cache (by default)](#anchor4)
&nbsp;  
&nbsp;  
 

<a name="anchor1"></a>
# 1. A brief intro on Docker and Image Size 
<img src="/Docs/docker_small.png" width="400">  

[photo cred](https://learnk8s.io/blog/smaller-docker-images/) 

### - A docker *image* is the "base" of the docker container. 
### - A docker *image* is created by a *Dockerfile*, which is a set of instructions that act as a multi-layered filesystem.
### - When Docker runs the *image* it will produce one (or many) containers.
&nbsp;  
## Formula for Docker Image Size
### Image Size = Base Image + Essential Files + *Cruft* (a.k.a. random, unneeded files)
<img src="/Docs/cruft.jpg" width="450">  

[photo cred](https://slangit.com/meaning/cruft)  
&nbsp;  
### Why you should avoid large Docker images:
1. First and foremost: it’s *best practice* to maintain a small image size [Docker Docs](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
2. Large images increase image build time (detrimental to continuous integration and continuous development/deployment)
3. Large images also including pushing/pulling time (detrimental to continuous integration and continuous development/deployment)
4. Reducing unnecesary dependencies will decrease both the complexity and the chances of vulnerability in your application


<a name="anchor2"></a>
# 2. Ways to reduce the size of a docker image
<img src="/Docs/slim_docker.png" width="450">  

[photo cred](https://towardsdatascience.com/how-to-build-slim-docker-images-fast-ecc246d7f4a7/)  

* smaller base image
* fewer layers (combine commands, reduce use of "RUN")
* Use .dockerignore
* Add rm -rf /var/lib/apt/lists/* at the end of the apt-get -y (removes package manager cache)  
* Remove unnecessary dependencies with -–no-install-recommends flag
* Some tools, like [dive](https://github.com/wagoodman/dive), can help find heavy layers and look exactly at files that are being added in each layer 
* Something I recently started using: *multi-stage*, described a bit more below


# 3.  Docker multi-staging to the rescue

Docker image layers are sort of like git commits, they store the difference between the previous and current version. 

However, layers use space, and the more layers that you have, the heavier your final image will be. Git repositories are similar, as the size of your repo increases with the number of layers (Git has to store all the changes between commits).

Each stage begins with a FROM instruction. The required artifact passes to the following stage, leaving behind content that you won’t need in the final image artifact.




# 4. A note on AWS and its lack of cache (by default)

By default, AWS does not utilize cache. There are ways to turn it on (ask Josh :) ), but I have found that with a large build using multistaging can be useful.
