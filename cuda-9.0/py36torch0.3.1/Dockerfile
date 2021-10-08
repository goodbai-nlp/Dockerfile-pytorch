FROM nvidia/cuda:9.0-base-ubuntu16.04

# Install some basic utilities
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone && \
    apt-get update && apt-get install -y \
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
    language-pack-zh-hans && \
    rm -rf /var/lib/apt/lists/* && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    echo -e "LANG=\"zh_CN.UTF-8\"\nLANGUAGE=\"zh_CN:zh:en_US:en\"" >>/etc/environment && \
    echo -e "en_US.UTF-8 UTF-8\nzh_CN.UTF-8 UTF-8\nzh_CN.GBK GBK\nzh_CN GB2312" >>/var/lib/locales/supported.d/local && \
    locale-gen

# Create a working directory
RUN mkdir /workspace
WORKDIR /workspace

# clone vim configs
RUN git clone https://github.com/muyeby/vim-config.git \
 && curl -o /root/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-4.5.1-Linux-x86_64.sh \
 && chmod +x /root/miniconda.sh \
 && /root/miniconda.sh -b -p /opt/conda \
 && rm /root/miniconda.sh \
 && /opt/conda/bin/conda install h5py matplotlib tqdm scikit-learn gensim nltk \
 && /opt/conda/bin/conda install -y -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/linux-64/ cuda90 pytorch=0.3.1 \
 && /opt/conda/bin/pip install h5py-cache torchtext configargparse \
 && conda clean -ya

ENV PATH=/root/miniconda/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Set the default command to python3
CMD ["python3"]
