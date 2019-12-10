FROM nvidia/cuda:9.0-base-ubuntu16.04

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
RUN curl -so ~/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-4.5.11-Linux-x86_64.sh \
 && chmod +x ~/miniconda.sh \
 && ~/miniconda.sh -b -p ~/miniconda \
 && rm ~/miniconda.sh
ENV PATH=/root/miniconda/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Create a Python 3.6 environment
RUN ~/miniconda/bin/conda install conda-build \
 && ~/miniconda/bin/conda create -y --name py36 python=3.6.8 \
 && ~/miniconda/bin/conda clean -ya
ENV CONDA_DEFAULT_ENV=py36
ENV CONDA_PREFIX=/root/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH

# CUDA 9.0-specific steps
RUN ~/miniconda/bin/conda install -y -c pytorch \
    cuda90=1.0 \
    magma-cuda90=2.4.0 \
    "pytorch=1.0.0=py3.6_cuda9.0.176_cudnn7.4.1_1" \
    torchvision=0.2.1 \
 && ~/miniconda/bin/conda clean -ya

# Install HDF5 Python bindings
RUN ~/miniconda/bin/conda install -y h5py=2.8.0 matplotlib tqdm scikit-learn gensim \
 && ~/miniconda/bin/conda clean -ya
RUN pip install h5py-cache==1.0

# Install Requests, a Python library for making HTTP requests
RUN ~/miniconda/bin/conda install -y requests=2.19.1 \
 && ~/miniconda/bin/conda clean -ya

# Set the default command to python3
CMD ["python3"]
