Bootstrap: docker
From: nvidia/cuda:8.0-cudnn7-runtime-ubuntu16.04

# this command assumes singularity 2.3
%environment
    PATH="/usr/local/anaconda/bin:$PATH"
    CUDNN_VERSION=7.2.1.38
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
    
    echo "deb https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list


    com.nvidia.cudnn.version="${CUDNN_VERSION}"
    apt-get update && apt-get install -y --no-install-recommends \
            libcudnn7=$CUDNN_VERSION-1+cuda8.0 \
            libcudnn7-dev=$CUDNN_VERSION-1+cuda8.0 && \
    apt-mark hold libcudnn7 && \
    rm -rf /var/lib/apt/lists/*


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
