Bootstrap: docker
From: nvidia/cuda:8.0-cudnn7-runtime-ubuntu16.04

# this command assumes singularity 2.3
%environment
    PATH="/usr/local/anaconda/bin:$PATH"
%post
    apt-get update
    apt-get upgrade -y
     
    apt-get install -y eatmydata
    eatmydata apt-get install -y wget bzip2 \
      ca-certificates libglib2.0-0 libxext6 libsm6 libxrender1 \
      git git-annex
    #-standalone
    apt-get clean
    pip install --upgrade pip
    
     # download and run NIH HPC NVIDIA driver installer
    wget https://raw.githubusercontent.com/NIH-HPC/gpu4singularity/master/gpu4singularity
    chmod u+rwx gpu4singularity
    export VERSION=375.66
    ./gpu4singularity --verbose \
        -u http://us.download.nvidia.com/XFree86/Linux-x86_64/"${VERSION}"/NVIDIA-Linux-x86_64-"${VERSION}".run \
        -V "${VERSION}"
    rm gpu4singularity

    # install anaconda
    if [ ! -d /usr/local/anaconda ]; then
         wget https://repo.continuum.io/miniconda/Miniconda2-4.3.14-Linux-x86_64.sh \
            -O ~/anaconda.sh && \
         bash ~/anaconda.sh -b -p /usr/local/anaconda && \
         rm ~/anaconda.sh
    fi
    # set anaconda path
    export PATH="/usr/local/anaconda/bin:$PATH"

    # install the bare minimum
    conda install\
      numpy scipy
    conda install torchvision
    conda install -c anaconda pillow
    conda install -c pytorch pytorch=0.4.1 cuda80
    

    
    conda clean --tarballs

    # OpenCV from pip, including contrib.  This makes the install MUCH faster.
    # See https://pypi.python.org/pypi/opencv-contrib-python for capabilities 
    # and limitations.  
    pip install --no-cache-dir opencv-contrib-python
    
    # make /data and /scripts so we can mount it to access external resources
    if [ ! -d /data ]; then mkdir /data; fi
    if [ ! -d /scripts ]; then mkdir /scripts; fi

%runscript
    echo "Now inside Singularity container woah..."
    exec /bin/bash
