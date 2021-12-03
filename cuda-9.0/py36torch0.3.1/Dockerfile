FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04

# Install some basic utilities
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone

RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
    openssh-server \
    vim \
    htop \
    build-essential \
    tzdata \
    language-pack-zh-hans \
 && rm -rf /var/lib/apt/lists/*

RUN dpkg-reconfigure --frontend noninteractive tzdata

# FileEncoding and Language support
RUN echo -e "LANG=\"zh_CN.UTF-8\"\nLANGUAGE=\"zh_CN:zh:en_US:en\"" >>/etc/environment
RUN echo -e "en_US.UTF-8 UTF-8\nzh_CN.UTF-8 UTF-8\nzh_CN.GBK GBK\nzh_CN GB2312" >>/var/lib/locales/supported.d/local
RUN locale-gen

# Create a working directory
RUN mkdir /workspace
WORKDIR /workspace

# clone vim configs
RUN git clone https://github.com/muyeby/vim-config.git

# Install Miniconda
RUN curl -o /root/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-4.5.1-Linux-x86_64.sh \
 && chmod +x /root/miniconda.sh \
 && /root/miniconda.sh -b -p /root/miniconda \
 && rm /root/miniconda.sh
ENV PATH=/root/miniconda/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Create a Python 3.6 environment
RUN ~/miniconda/bin/conda create -y --name py36 python=3.6 \
 && ~/miniconda/bin/conda clean -ya
ENV CONDA_DEFAULT_ENV=py36
ENV CONDA_PREFIX=/root/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH
RUN /root/miniconda/bin/conda install conda-build \
 && /root/miniconda/bin/conda clean -ya

RUN conda install -y -c pytorch \
    cuda90 \
	pytorch=0.3.1 \
 && conda clean -ya

RUN conda install -y h5py matplotlib tqdm scikit-learn gensim nltk \
 && conda clean -ya
RUN pip install h5py-cache torchtext configargparse
# Install Requests, a Python library for making HTTP requests
RUN conda install -y requests=2.19.1 \
 && conda clean -ya

# Set the default command to python3
CMD ["python3"]
