FROM nvidia/cuda:10.2-cudnn7-devel-ubuntu20.04 AS BUILD

# LABEL maintainer="ceshine@ceshine.net"

# Based on https://github.com/anurag/fastai-course-1/

ARG PYTHON_VERSION=3.7
ARG CONDA_PYTHON_VERSION=3
ARG CONDA_DIR=/opt/conda

# Instal basic utilities
RUN apt-get update && \
    apt-get install -y --no-install-recommends git wget unzip bzip2 build-essential ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install miniconda
ENV PATH $CONDA_DIR/bin:$PATH
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda$CONDA_PYTHON_VERSION-latest-Linux-x86_64.sh -O /tmp/miniconda.sh && \
    echo 'export PATH=$CONDA_DIR/bin:$PATH' > /etc/profile.d/conda.sh && \
    /bin/bash /tmp/miniconda.sh -b -p $CONDA_DIR && \
    rm -rf /tmp/* && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN conda install -y python=$PYTHON_VERSION && \
    conda install -y pytorch=1.6 torchvision cudatoolkit=10.2 -c pytorch && \
    conda install -y h5py scikit-learn matplotlib seaborn tqdm gensim \
    pandas mkl-service cython && \
    conda clean -tipsy

# Install apex
WORKDIR /tmp/
RUN git clone https://github.com/NVIDIA/apex.git && \
    cd apex && pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" --global-option="--deprecated_fused_adam" --global-option="--xentropy" --global-option="--fast_multihead_attn" .

RUN  pip install --upgrade pip && \
    pip install pillow-simd h5py-cache torchtext configargparse && \
    rm -rf ~/.cache/pip

# Runtime image
FROM nvidia/cuda:10.2-base-ubuntu20.04

ARG CONDA_DIR=/opt/conda
ARG USERNAME=docker
ARG USERID=1000

RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone
# Instal basic utilities
RUN apt-get update && \
    apt-get install -y --no-install-recommends git wget unzip bzip2 sudo p7zip openssh-server vim htop tzdata language-pack-zh-hans curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
	dpkg-reconfigure --frontend noninteractive tzdata && \
	echo -e "LANG=\"zh_CN.UTF-8\"\nLANGUAGE=\"zh_CN:zh:en_US:en\"" >>/etc/environment && \
	echo -e "en_US.UTF-8 UTF-8\nzh_CN.UTF-8 UTF-8\nzh_CN.GBK GBK\nzh_CN GB2312" >>/var/lib/locales/supported.d/local && \
	locale-gen

ENV PATH $CONDA_DIR/bin:$PATH
ENV CUDA_HOME=/usr/local/cuda
ENV CUDA_ROOT=$CUDA_HOME
ENV PATH=$PATH:$CUDA_ROOT/bin:$HOME/bin
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_ROOT/lib64

RUN mkdir -p /opt/conda/ && \
    mkdir /workspace


# Create the user
#RUN useradd --create-home -s /bin/bash --no-user-group -u $USERID $USERNAME && \
#    chown $USERNAME $CONDA_DIR -R && \
#    adduser $USERNAME sudo && \
#    echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
#USER $USERNAME
WORKDIR /workspace


COPY --chown=1000 --from=build /opt/conda/. $CONDA_DIR
