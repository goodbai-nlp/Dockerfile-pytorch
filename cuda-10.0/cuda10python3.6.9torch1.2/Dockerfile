FROM nvidia/cuda:10.0-base-ubuntu16.04

# Install some basic utilities
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
 && rm -rf /var/lib/apt/lists/*

# Create a working directory
RUN mkdir /workspace
WORKDIR /workspace

# Install Miniconda
RUN curl -o /root/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
 && chmod +x ~/miniconda.sh \
 && ~/miniconda.sh -b -p ~/miniconda \
 && rm ~/miniconda.sh
ENV PATH=/root/miniconda/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Create a Python 3.6 environment
RUN /root/miniconda/bin/conda create -y --name py36 python=3.6.9 \
 && /root/miniconda/bin/conda clean -ya
ENV CONDA_DEFAULT_ENV=py36
ENV CONDA_PREFIX=/root/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH
RUN /root/miniconda/bin/conda install conda-build=3.18.9=py36_3 \
 && /root/miniconda/bin/conda clean -ya

# CUDA 10.0-specific steps
RUN conda install -y -c pytorch \
    cudatoolkit=10.0 \
    "pytorch=1.2.0=py3.6_cuda10.0.130_cudnn7.6.2_0" \
    "torchvision=0.4.0=py36_cu100" \
 && conda clean -ya

# Install HDF5 Python bindings
RUN conda install -y h5py=2.8.0 matplotlib tqdm scikit-learn gensim \
 && conda clean -ya
RUN pip install h5py-cache==1.0

# Install Requests, a Python library for making HTTP requests
RUN conda install -y requests=2.19.1 \
 && conda clean -ya

# Set the default command to python3
CMD ["python3"]
