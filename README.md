# Docker_multistaging
Multistaging with Docker to reduce complexity, size, and build time

<img src="/Docs/multistage.jpg" width="600">  

&nbsp;  

### [1.  A brief intro on Docker Image Size](#anchor1)
### [2.  Ways to reduce the size of a docker image](#anchor2)
### [3.  AWS and its lack of cache (by default)](#anchor3)
### [4.  Docker multi-staging to the rescue (maybe)](#anchor4)

&nbsp;  
&nbsp;  
 

<a name="anchor1"></a>
# 1. A brief intro on Docker Image Size
&nbsp;    

* A docker *image* is the "base" of the docker container. 
* A docker *image* is created by a *Dockerfile*, which is a set of instructions that are executed to make the docker *image*

