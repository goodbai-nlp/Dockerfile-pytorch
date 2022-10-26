FROM nvidia/cuda:10.2-devel-ubuntu16.04

ARG CONDA_PYTHON_VERSION=3
ARG CONDA_DIR=/opt/conda
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone

# Instal basic utilities
RUN apt-get update && \
    apt-get install -y --no-install-recommends git wget unzip bzip2 p7zip build-essential ca-certificates openssh-server vim htop zsh tzdata language-pack-zh-hans curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
	dpkg-reconfigure --frontend noninteractive tzdata && \
	echo "LANG=\"zh_CN.UTF-8\"\nLANGUAGE=\"zh_CN:zh:en_US:en\"" >>/etc/environment && \
    echo "en_US.UTF-8 UTF-8\nzh_CN.UTF-8 UTF-8\nzh_CN.GBK GBK\nzh_CN GB2312" >>/var/lib/locales/supported.d/local && \
	locale-gen

ENV LANG en_US.utf8

COPY environment.yml /tmp/environment.yml
# Install miniconda
ENV PATH $CONDA_DIR/bin:$PATH
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda$CONDA_PYTHON_VERSION-latest-Linux-x86_64.sh -O /tmp/miniconda.sh && \
    echo 'export PATH=$CONDA_DIR/bin:$PATH' > /etc/profile.d/conda.sh && \
    /bin/bash /tmp/miniconda.sh -b -p $CONDA_DIR && \
    conda env update -n base -f /tmp/environment.yml && \
    rm -rf /tmp/* && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    conda clean -ya && \
    rm -rf ~/.cache/pip

RUN mkdir /workspace
WORKDIR /workspace/
# Set the default command to python3
CMD ["python3"]
