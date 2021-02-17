# Docker_multistaging
#### Multistaging with Docker to reduce complexity, size, and build time

<img src="/Docs/multistage.jpg" width="700">  

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
<img src="/Docs/docker_small.png" width="500">  

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
*For more detailed info on Docker, check out: https://github.com/sdl002/R_Docker_Intro*
&nbsp;  
### Why you should avoid large Docker images:
1. First and foremost: it’s *best practice* to maintain a small image size (see [Docker Docs](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/))
2. Large images increase image build time (detrimental to continuous integration and continuous development/deployment)
3. Large images also including pushing/pulling time (detrimental to continuous integration and continuous development/deployment)
4. Reducing unnecesary dependencies will decrease both the complexity and the chances of vulnerability in your application

&nbsp;  
&nbsp;  

Real world example of a painfully large Docker Image:


<img src="/Docs/example.pdf" width="850">  





<a name="anchor2"></a>
# 2. Ways to reduce the size of a docker image
<img src="/Docs/slim_docker.png" width="450">  

[photo cred](https://towardsdatascience.com/how-to-build-slim-docker-images-fast-ecc246d7f4a7/)  
&nbsp;  

### 1. smaller base image
<img src="/Docs/Alpine_logo.png" width="450">  

&nbsp;  

### 2. fewer layers (combine commands, reduce use of "RUN")
<img src="/Docs/example_combine_commands.png" width="750">    

[photo cred](https://www.cloudbees.com/blog/reduce-docker-image-size/)   
&nbsp;  

### 3. Use .dockerignore
<img src="/Docs/dockeringore.png" width="750">   

[photo cred](https://medium.com/bb-tutorials-and-thoughts/docker-a-beginners-guide-to-dockerfile-with-a-sample-project-6c1ac1f17490)   
&nbsp;  


### 4. Add rm -rf /var/lib/apt/lists/* at the end of the apt-get -y (removes package manager cache)  
<img src="/Docs/clean_up.png" width="750">  

[photo cred](https://phoenixnap.com/kb/docker-image-size)
&nbsp;  

### 5. Remove unnecessary dependencies with -–no-install-recommends flag
<img src="/Docs/no_install_reccomends.png" width="750">   

[photo cred](http://garywiz.github.io/chaperone/guide/chap-docker-smaller.html)
&nbsp;  

### 6. Some tools, like [dive](https://github.com/wagoodman/dive), can help find heavy layers and show what files are being added in each layer 
<img src="/Docs/dive.png" width="750">   

[photo cred](https://github.com/wagoodman/dive)
&nbsp;  

### 7. Something I recently started using: *multi-stage*, described a bit more below
<img src="/Docs/mutli_stage.png" width="750">   

[photo cred](https://aboullaite.me/multi-stage-docker-java/)
&nbsp;

# 3.  Docker multi-staging to the rescue

### The FROM statement:
#### When building a Docker image, each stage begins with a "FROM" instruction. Your Dockerfile can have multiple "FROM" statements, and you can chose what artifacts pass through to the next stage (removing potential *CRUFT*)

#### When using multi-stage, you can include multiple stages in the same Dockerfile (as shown in the below example from [Docker Docs](https://docs.docker.com/develop/develop-images/multistage-build/))
<img src="/Docs/docker_docks_mulit.png" width="950">   

[photo cred](https://docs.docker.com/develop/develop-images/multistage-build/)
&nbsp;

#### You can also use external images, and copy "FROM" them into your production image:


# 4. A note on AWS and its lack of cache (by default)

By default, AWS does not utilize cache when buidling Docker images. There are ways to turn it on (ask Josh :) ), but I have found that with a large build using multistaging can also be useful.
&nbsp;
See some options for using cache with AWS (and also probably still check with Josh):
<img src="/Docs/cache1.png" width="1100">   
<img src="/Docs/cache2.png" width="1100"> 
<img src="/Docs/cache3.png" width="1100"> 
Reference: https://docs.aws.amazon.com/codebuild/latest/userguide/build-caching.html
&nbsp;
### I was able to somewhat circumvent giant image issues by utilzing multi-staging, and the other methods listed above, although my image could defnitely still be further optimizated and could use some more fine tuning. That being said, my build time and final image size was reduced by >50%... saving ~25 minutes in build time. Which is very helpful when testing an application.

&nbsp;

<img src="/Docs/testing.gif" width="800">  
