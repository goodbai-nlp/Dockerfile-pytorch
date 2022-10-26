FROM nvidia/cuda:11.3.1-devel-ubuntu20.04 AS BUILD

# LABEL maintainer="muyeby"

ARG CONDA_PYTHON_VERSION=3
ARG CONDA_DIR=/opt/conda

RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone
# Instal basic utilities
RUN apt-get update && \
    apt-get install -y --no-install-recommends git wget unzip bzip2 sudo p7zip openssh-server vim htop tzdata language-pack-zh-hans curl zsh && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
	dpkg-reconfigure --frontend noninteractive tzdata && \
	echo "LANG=\"zh_CN.UTF-8\"\nLANGUAGE=\"zh_CN:zh:en_US:en\"" >>/etc/environment && \
	echo "en_US.UTF-8 UTF-8\nzh_CN.UTF-8 UTF-8\nzh_CN.GBK GBK\nzh_CN GB2312" >>/var/lib/locales/supported.d/local && \
	locale-gen

WORKDIR /tmp/
COPY environment.yml /tmp/environment.yml
# Install miniconda
ENV PATH $CONDA_DIR/bin:$PATH
ENV CUDA_HOME=/usr/local/cuda
ENV CUDA_ROOT=$CUDA_HOME
ENV PATH=$PATH:$CUDA_ROOT/bin:$HOME/bin
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_ROOT/lib64

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda$CONDA_PYTHON_VERSION-latest-Linux-x86_64.sh -O /tmp/miniconda.sh && \
    echo 'export PATH=$CONDA_DIR/bin:$PATH' > /etc/profile.d/conda.sh && \
    /bin/bash /tmp/miniconda.sh -b -p $CONDA_DIR && \
	conda env update -n base -f /tmp/environment.yml && \
    rm -rf /tmp/* && \
    apt-get clean && \
    conda clean -y -a

RUN mkdir -p /opt/conda/ && \
    mkdir /workspace

WORKDIR /workspace

