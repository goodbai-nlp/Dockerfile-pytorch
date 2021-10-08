FROM klauskyj/cuda8.0-cudnn7:v1.0

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
	language-pack-zh-hans \
 && rm -rf /var/lib/apt/lists/* \
 && dpkg-reconfigure --frontend noninteractive tzdata \
 && echo -e "LANG=\"zh_CN.UTF-8\"\nLANGUAGE=\"zh_CN:zh:en_US:en\"" >>/etc/environment \
 && echo -e "en_US.UTF-8 UTF-8\nzh_CN.UTF-8 UTF-8\nzh_CN.GBK GBK\nzh_CN GB2312" >>/var/lib/locales/supported.d/local \
 && locale-gen

# Create a working directory
RUN mkdir /workspace
WORKDIR /workspace

# clone vim configs
RUN git clone https://github.com/muyeby/vim-config.git \
 && curl -o /root/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
 && chmod +x /root/miniconda.sh \
 && /root/miniconda.sh -b -p /root/miniconda \
 && rm /root/miniconda.sh
ENV PATH=/root/miniconda/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Create a Python 3.7 environment
RUN ~/miniconda/bin/conda create -y --name py36 python=3.6 \
 && ~/miniconda/bin/conda clean -ya
ENV CONDA_DEFAULT_ENV=py36
ENV CONDA_PREFIX=/root/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH


# CUDA 8.0-specific steps
RUN conda install -y -c pytorch \
    cuda80 \
    magma-cuda80 \
    pytorch=0.3.0 \
    h5py matplotlib tqdm scikit-learn gensim nltk networkx=2.3 \
 && conda clean -ya \
 && pip install h5py-cache torchtext configargparse pycorenlp parsimonious \
 && conda install -y requests=2.19.1 \
 && conda clean -ya

# Set the default command to python3
CMD ["python3"]
