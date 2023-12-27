#FROM pytorch/pytorch:2.1.0-cuda11.8-cudnn8-devel
FROM   nvidia/cuda:11.0.3-devel-ubuntu20.04

RUN apt-get update \
    && apt-get install -y build-essential \
    wget git vim sudo 
    # \
    # && apt-get clean \
    # && rm -rf /var/lib/apt/lists/*
    

ENV CONDA_DIR /opt/conda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-py38_23.10.0-1-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda

ENV PATH=$CONDA_DIR/bin:$PATH
RUN conda install -y -c fvcore -c iopath -c conda-forge fvcore iopath

RUN echo '==========================='
RUN pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio==0.7.2 -f https://download.pytorch.org/whl/torch_stable.html

RUN conda install -c conda-forge python-devtools

RUN set -x && cat /etc/apt/sources.list 
RUN perl -i.bak -pe 's#\/archive\.ubuntu\.com#\/tw\.archive\.ubuntu\.com#g'  /etc/apt/sources.list
RUN apt-get update &&  apt install -y build-essential libopenblas-dev

RUN apt-get update && apt-get install -y \
    ninja-build \
    libxi-dev \  
    # Headless rendering
    libosmesa6-dev 


WORKDIR /tools

#RUN pip install -U git+https://github.com/NVIDIA/MinkowskiEngine 
RUN pip install -U git+https://github.com/NVIDIA/MinkowskiEngine -v --no-deps 

# RUN git clone --recursive git@github.com:pengsongyou/openscene.git
RUN git clone  https://github.com/pengsongyou/openscene.git

WORKDIR /tools/openscene
RUN pip install -r requirements.txt


# RUN apt-get update && apt-get install - y libglew-dev 
RUN apt-get update && apt-get install -y libglew-dev 
RUN apt-get update && apt-get install -y x11-apps 


RUN pip install jupyterlab

#=========================

ARG UID
ARG GID 
RUN useradd -rm -l -s /bin/bash dkuser -G sudo -u ${UID} 
RUN groupadd -g ${GID} ugroup

RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER dkuser 

WORKDIR /work 
CMD ["/bin/bash"]