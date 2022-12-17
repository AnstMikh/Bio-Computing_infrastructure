# 1 - Dependencies management

***git branch name:*** dependencies

## Theory \[2\]

As usual, we will start with a few theoretical questions:

-   \[0.5\] What is Docker, and how it differs from dependencies
    management systems? From virtual machines?

Docker is an open source platform for building, deploying, and managing
containerized applications. Docker is different form the other
dependencies management system due to its flexibility and security. The
Docker Hub is the company\'s cloud-hosted service that offers over
100,000 free apps, public, and private registries, with official
repositories from leading third party vendors---from Nginx and Ubuntu to
MongoDB and Redis. Also it is designed to be lightweight and simple.
Docker has a good security rate 760. Docker is very popular to use, so
if you get a bug, there\'s a 0.99 chance you\'ll find a solution online,
and pretty quickly.

While each workload on a virtual machine (VM) requires a full OS or
hypervisor, many workloads can run on a single OS when using Docker.
Docker containers have a speedy startup time and shorten boot-up times.
VMs take a while to start up and have terrible performance. Because
Docker containers are smaller than VMs, it is simpler to move files to
the host\'s filesystem. (VMs should have a copy of the OS and all of its
dependencies; otherwise, image size will increase and data sharing will
be laborious.) Due to the OS being operational, Docker containers\'
applications launch immediately. Virtual Machines must launch the
complete OS in order to install a single program, which requires a full
boot process. Docker can have multiple tenats and most amenities (binary
and library) are shared with neighbors (applications). While VM does not
have these properties.

------------------------------------------------------------------------

-   \[0.5\] What are the advantages and disadvantages of using
    containers over other approaches?

**Advantages of Containers**

1.  Consistency (containers work the same way across multiple
    environments)
2.  Automation (containers make it possible to plan a variety of tasks
    to happen when they are needed without a person having to manually
    intervene.)
3.  Stability
4.  Saves Space (Containers are significantly smaller and do not require
    massive, physical servers because they run solely on the cloud and
    use the app\'s code and its dependencies.)

**Disadvantages of Containers**

1.  Advances Quickly (associated documentation doesn't always update
    quite as quickly as the technology itself)
2.  Learning Curve
3.  Hard to get rid of :)

------------------------------------------------------------------------

-   \[0.5\] Explain how Docker works: what are Dockerfiles, how are
    containers created, and how are they run and destroyed?

**Dockerfile**

Dockerfile is a text document that contains all the commands a user
could call on the command line to assemble an image.

**Сreate a directory in order to store all the Docker images that will
be builded**

    mkdir docker_dir_name #create a directory
    cd docker_dir_name  #move to it
    touch Dockerfile
    nano Dockerfile #open the file in text editor

Then, add the following content:

    FROM ubuntu
    MAINTAINER author
    RUN apt-get update
    CMD ["echo", "Welcome to the container"]

ctrl+C, ctrl+X

**Build a Docker Image with Dockerfile**

    docker build [OPTIONS] PATH | URL | - #declare the path where we will be storing the dockerfile docker_dir_name
    docker build [location of your dockerfile] #build a basic image using a Dockerfile
    docker build -t image_name #to make the new image be tagged with a name
    docker images #verify the creation

The output should show the image is available in the repository.

**Create a New Container**

    docker run [OPTIONS] IMAGE [COMMAND] [ARG...] #run a command in a new container (reates a writeable container layer over the specified image, and then starts it using the specified command.)

**Destroy it**

    docker kill [OPTIONS] CONTAINER [CONTAINER...] #kill one or more running containers (By default SIGKILL signal is sended but can be customized)

------------------------------------------------------------------------

-   \[0.25\] Name and describe at least one Docker competitor (i.e., a
    tool based on the same containerization technology).

**CoreOS Rkt**

A containerization software engine called CoreOS rkt (pronounced
\"rocket\") allows you to run application workloads separately from the
underlying infrastructure. It is the Docker container engine\'s main
rival.

The Application Container Image (ACI) from the App Container spec serves
as the foundation for CoreOS rkt (appc). An ACI is a tarball made of of
an image manifest and a root file system with all the files required to
run an application. The majority of the default settings can be changed
at runtime, and the rkt run command lets users add their own execution
parameters to a specific image.

The majority of the major Linux OS versions support CoreOS rkt
containers as a binary file that integrates with Linux init systems and
scripts and runs in accordance with the parent-child Unix process model.

Pods, which are collections of concurrently running programs, are used
by CoreOS rkt for container configuration. A pod may include distinct
configurations for various scopes, from the pod level all the way down
to specific applications within the pod. Pods operate independently and
in remote locations without a central daemon.

------------------------------------------------------------------------

-   \[0.25\] What is conda? How it differs from apt, yarn, and others?

Conda is an open source package management system and environment
management system that runs on Windows, macOS, Linux and z/OS. Conda
quickly installs, runs and updates packages and their dependencies.
Conda easily creates, saves, loads and switches between environments on
your local computer. It was created for Python programs, but it can
package and distribute software for any language.

Conda is more popular for developers needs (has a lot of developer tools
comparing to yarn). It allows the user to work localy in environments
not in the root/bin directory for example, the user can use different
python versions in each env (conda vs apt).

## Problem \[6.5\]

The problem itself is relatively simple.

Imagine that you developed an excellent RNA-seq analysis pipeline and
want to share it with the world. Based on your experience, you are
confident that the popularity of the pipeline will be proportional to
its ease of use. So, you decided to help your future users and to pack
all dependencies in a Conda environment and a Docker container.

Here is the list of tools and their versions that are used in your work:

-   [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/),
    v0.11.9
-   [STAR](https://github.com/alexdobin/STAR), v2.7.10b
-   [reat](https://github.com/alnfedorov/reat), commit
    ee19bc928badd227975410a6b8b715c0e03bd4ab
-   [samtools](https://github.com/samtools/samtools), v1.16.1
-   [picard](https://github.com/broadinstitute/picard), v2.27.5
-   [salmon](https://github.com/COMBINE-lab/salmon), commit tag 1.9.0
-   [bedtools](https://github.com/arq5x/bedtools2), v2.30.0
-   [multiqc](https://github.com/ewels/MultiQC), v1.13

**Anaconda**:

-   \[1\] Install conda, create a new virtual environment, and install
    all necessary packages.
-   \[0.75\] You won\'t be able to install some tools - that\'s fine.
    List their names, and explain what should be done to make them
    conda-friendly
    ([conda-forge](https://conda-forge.org/docs/maintainer/adding_pkgs.html)
    channel,
    [bioconda](https://bioconda.github.io/contributor/workflow.html)
    channel).
-   \[0.25\]
    [Export](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#exporting-the-environment-yml-file)
    the environment
    ([example](https://github.com/nf-core/clipseq/blob/master/environment.yml))
    to the file and verify that it can be
    [rebuilt](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file)
    from the file without problems.

**Docker**:

-   \[3\] Create a Dockerfile for a container with **all** required
    dependencies. Don\'t forget about comments; test that all tools are
    accessible and work inside the container. Hints:
-   If needed, grant rights to execute downloaded/compiled binaries
    using chmod (`chmod a+x BINARY_NAME`)
-   Move all executables to \$PATH folders (e.g.`/usr/local/bin`) to
    make them accessible without specifying the full path.
-   Typical command to run a container interactively (`-it`) and delete
    on exit(`--rm`): `docker run --rm -it name:tag`
-   \[1\] Use [hadolint](https://hadolint.github.io/hadolint/) and
    remove as many reported warnings as possible.
-   \[0.5\] Add relevant
    [labels](https://docs.docker.com/engine/reference/builder/#label),
    e.g. maintainer, version, etc.
    ([hint](https://medium.com/@chamilad/lets-make-your-docker-image-better-than-90-of-existing-ones-8b1e5de950d))

### Anaconda

**Install anaconda and create env**

    bash ~/Anaconda3-2022.10-Linux-x86_64.sh
    conda create -n p2 --no-default-packages -y
    conda activate p2

**Add channels**

I added channels default fror searching packages in default repositories
(<https://repo.anaconda.com/pkgs/>) and conda-forge in case of using
Git-Hub repositories

    conda config --add channels defaults
    conda config --add channels conda-forge
    conda config --add channels bioconda

**Install packages**

All these packages need channel bioconda since they stored
<https://bioconda.github.io/conda-package_index.html>

Now conda is friendly :)

    conda install -y samtools=1.16.1 
    conda install -y picard
    conda install -y fastqc=0.11.9
    conda install -y bedtools=2.30.0
    conda install -y multiqc=1.13
    conda install -y star=2.7.10b
    conda install -y salmon=1.9.0

I could not specify the version of picard, so I just installed the
avaliable version of it

**Export the env and rebuilting** Export env into env_list and try to
rebuild the env from the file (by last 2 rows)

    conda env export --from-history > environment.yml
    conda deactivate
    conda remove --name myenv --all -y
    conda env create -f environment.yml

#### Enviroment.yml

    name: p2
    channels:
      - conda-forge
      - bioconda
      - defaults
    dependencies:
      - samtools=1.16.1
      - picard
      - fastqc=0.11.9
      - bedtools=2.30.0
      - multiqc=1.13
      - star=2.7.10b
      - salmon=1.9.0

    prefix: /home/anst/anaconda3/envs/p2

### Docker

I used this way of installing docker since only it worked for me.

### Install Docker

    sudo apt update
    sudo apt upgrade
    sudo apt install docker.io
    sudo snap install docker

Some useful links:

1.  <https://www.simplilearn.com/tutorials/docker-tutorial/how-to-install-docker-on-ubuntu>
2.  <https://gist.github.com/alyleite/ca8b10581dbecd722d9dcc35b50d9b2b>

```
touch Dockerfile
nano Dockerfile
```

#### Dockerfile

    FROM ubuntu:20.04
    MAINTAINER "aamikhaylova_9@edu.hse.ru"

    #no interactive dialogues
    ARG DEBIAN_FRONTEND=noninteractive

    #update links and install apt-utils apt-transport-https unzip and python3-pip
    RUN apt-get update && apt -y install apt-utils apt-transport-https openjdk-11-jre-headless unzip python3-pip

    #create /.bashrc for aliases
    RUN touch /.bashrc

    #samtools v1.16.1
    RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip && \
        unzip samtools-1.16.1.zip && \
        rm samtools-1.16.1.zip && \
        mv samtools-1.16.1/misc samtools && \
        rm -r samtools-1.16.1 && \
        echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

    #picard v2.27.5
    RUN wget https://github.com/broadinstitute/picard/releases/download/2.27.5/picard.jar -O /bin/picard.jar && \
        chmod a+x /bin/picard.jar && \
        echo 'alias picard="java -jar /bin/picard.jar"' >> /.bashrc

    #fastqc v0.11.9
    RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip && \
        unzip fastqc_v0.11.9.zip && \
        rm fastqc_v0.11.9.zip && \
        chmod a+x FastQC/fastqc && \
        echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

    # bedtools v.2.30.0
    RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /bin/bedtools.static.binary && \
        chmod a+x /bin/bedtools.static.binary && \
        echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

    #multic v1.13
    RUN pip install multiqc==1.13

    #STAR v.2.7.10b
    RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip && \
        unzip STAR_2.7.10b.zip && \
        rm STAR_2.7.10b.zip && \
        chmod a+x STAR_2.7.10b/Linux_x86_64_static/STAR && \
        mv STAR_2.7.10b/Linux_x86_64_static/STAR /bin/STAR && \
        rm -r STAR_2.7.10b

    #salmon v.1.9.0
    RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz && \
        tar -zxvf salmon-1.9.0_linux_x86_64.tar.gz && \
        rm salmon-1.9.0_linux_x86_64.tar.gz && \
        chmod a+x salmon-1.9.0_linux_x86_64/bin/salmon && \
        mv salmon-1.9.0_linux_x86_64/bin/salmon /bin/salmon && \
        rm -r salmon-1.9.0_linux_x86_64 

**Build docker image** using flag \--no-cache in order to disable
caching. Run the container and make sure that it will be deleted after
**exit**.

     sudo docker build --no-cache -t ubuntu:20.04 .
     sudo docker run --rm -it ubuntu:20.04

**The process of changing Dockerfile**

I used Linter and removed all the report by such steps

1.  MAINTAINER \--\> LABEL maintainer
2.  Delete the apt-get lists after installing something
3.  apt \--\> apt-get install package=\"package_vesrion\"

Also I added some LABELs for version, name and description

#### Linter Labeled Optimazed Dockerfile

    FROM ubuntu:20.04
    LABEL maintainer="anastasia.mikhailova"
    ARG DEBIAN_FRONTEND=noninteractive

    LABEL org.label-schema.schema-version="1.0"
    LABEL org.label-schema.name="hw1"
    LABEL org.label-schema.description="Homework1 for Computing Infrastructure in Bioinformatics Problems by Anastasia Mikhailova"
    LABEL org.label-schema.docker.cmd="docker run -it --rm ubuntu:20.04 ."

    # apt-utils + apt-transport-https + python-pip + unzip
    RUN apt-get update && apt-get -y --no-install-recommends install apt-utils=2.4.8 apt-transport-https=2.0.2ubuntu0.2 && \
      apt-get -y --no-install-recommends install openjdk-11-jre-headless=11.0.17+8-1ubuntu2~20.04
      apt-get -y --no-install-recommends install python3-pip=20.0.2-5ubuntu1.5 unzip=6.0-25ubuntu1.1&& \ 
      apt-get clean && \ 
      rm -rf /var/lib/apt/lists/*

    # Create /.bashrc for aliases
    RUN touch /.bashrc

    # samtools v1.16.1
    RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip && \
        unzip samtools-1.16.1.zip && \
        rm samtools-1.16.1.zip && \
        mv samtools-1.16.1/misc samtools && \
        rm -r samtools-1.16.1 && \
        echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

    # picard v2.27.5
    RUN wget https://github.com/broadinstitute/picard/releases/download/2.27.5/picard.jar -O /bin/picard.jar && \
        chmod a+x /bin/picard.jar && \
        echo 'alias picard="java -jar /bin/picard.jar"' >> /.bashrc

    # fastqc v0.11.9
    RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip && \
        unzip fastqc_v0.11.9.zip && \
        rm fastqc_v0.11.9.zip && \
        chmod a+x FastQC/fastqc && \
        echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

    # bedtools v.2.30.0
    RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /bin/bedtools.static.binary && \
        chmod a+x /bin/bedtools.static.binary && \
        echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

    # multic v1.13
    RUN pip install multiqc==1.13

    # STAR v.2.7.10b
    RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip && \
        unzip STAR_2.7.10b.zip && \
        rm STAR_2.7.10b.zip && \
        chmod a+x STAR_2.7.10b/Linux_x86_64_static/STAR && \
        mv STAR_2.7.10b/Linux_x86_64_static/STAR /bin/STAR && \
        rm -r STAR_2.7.10b

    # salmon v.1.9.0
    RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz && \
        tar -zxvf salmon-1.9.0_linux_x86_64.tar.gz && \
        rm salmon-1.9.0_linux_x86_64.tar.gz && \
        chmod a+x salmon-1.9.0_linux_x86_64/bin/salmon && \
        mv salmon-1.9.0_linux_x86_64/bin/salmon /bin/salmon && \
        rm -r salmon-1.9.0_linux_x86_64 

## Extra points \[1.5\]

You will be awarded extra points for the following:

-   \[0.5\] Using [multi-stage
    builds](https://docs.docker.com/build/building/multi-stage/) in
    Docker. E.g. to build reat and copy only the executable to the final
    image.

-   \[0.75\] Minimizing the size of the final Docker image. That is,
    removing all intermediates, unnecessary binaries/caches, etc. Don\'t
    forget to compare & report the final size before and after all the
    optimizations.

-   \[0.25\] Create an extra Dockerfile that starts from [a conda base
    image](https://hub.docker.com/r/continuumio/anaconda3) and builds
    everything from your conda environment file.

Hint: `conda env create --quiet -f environment.yml && conda clean -a`
([example](https://github.com/nf-core/clipseq/blob/master/Dockerfile))

Docker image:

-   via raw Dockerfile \-- **1.86Gb**
-   via optimized Dockerfile \-- **583Mb**

#### Dockerfile for conda

    FROM continuumio/miniconda3
    LABEL maintainer="anastasia.mikhailova"
    #Labels.   
    LABEL org.label-schema.schema-version="1.0" 
    LABEL org.label-schema.name="hw1"
    LABEL org.label-schema.description="Homework1 by Anastasia Mikhailova"
    LABEL org.label-schema.docker.cmd="docker run -it --rm cconda ." 
                                                                           
    #create the env    
    COPY environment.yml . 
    RUN apt-get update && apt-get -y --no-install-recommends install apt-utils=2.4.8 apt-transport-https=2.0.2ubuntu0.2 && \
        conda env create --quiet -f environment.yml && conda clean -a  

Personally, Docker is quite ambiguous it is complicated to figure out the proper way of building images and particularly Dockerfiles. I still think that it maybe easier to build conda "in an old way" like *conda install*. However, if I have to reinstall my ubuntu (hope that no) or use somebody's computer, it would be nice to have a container with installed tools that I might use during my work.
