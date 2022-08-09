FROM pytorch/pytorch:1.11.0-cuda11.3-cudnn8-devel AS BUILD

# LABEL maintainer="muyeby"

ARG CONDA_PYTHON_VERSION=3
ARG CONDA_DIR=/opt/conda

# Instal basic utilities
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone
RUN apt-key del 7fa2af80 && \
    apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub && \
    apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu2004/x86_64/7fa2af80.pub && \
    apt-get update && \
    apt-get install -y --no-install-recommends git wget unzip bzip2 p7zip build-essential ca-certificates openssh-server vim htop zsh tzdata language-pack-zh-hans curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    echo "LANG=\"zh_CN.UTF-8\"\nLANGUAGE=\"zh_CN:zh:en_US:en\"" >> /etc/environment && \
    echo "en_US.UTF-8 UTF-8\nzh_CN.UTF-8 UTF-8\nzh_CN.GBK GBK\nzh_CN GB2312" >> /var/lib/locales/supported.d/local && \
    locale-gen

# Install miniconda
COPY environment.yml /tmp/environment.yml
# Install miniconda
ENV PATH $CONDA_DIR/bin:$PATH
RUN conda env update -n base -f /tmp/environment.yml

RUN git clone https://github.com/NVIDIA/apex.git && \
    cd apex && pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" --global-option="--deprecated_fused_adam" --global-option="--xentropy" --global-option="--fast_multihead_attn" . && \
    rm -rf /tmp/* && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    conda clean -ya && \
    rm -rf ~/.cache/pip