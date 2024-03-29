## PyTorch Docker image

[![Docker Automated build](https://img.shields.io/docker/automated/anibali/pytorch.svg)](https://hub.docker.com/r/muyeby/docker-pytorch)

Ubuntu + PyTorch + CUDA (optional)

This respository is a fork of [anibali/pytorch/](https://github.com/anibali/docker-pytorch.git)

### Requirements

In order to use this image you must have Docker Engine installed. Instructions
for setting up Docker Engine are
[available on the Docker website](https://docs.docker.com/engine/installation/).

If you have a CUDA-compatible NVIDIA graphics card, you can use a CUDA-enabled
version of the PyTorch image to enable hardware acceleration. I have only
tested this in Ubuntu Linux.

Firstly, ensure that you install the appropriate NVIDIA drivers and libraries.
If you are running Ubuntu, you can install proprietary NVIDIA drivers
[from the PPA](https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa)
and CUDA [from the NVIDIA website](https://developer.nvidia.com/cuda-downloads).

You will also need to install `nvidia-docker2` to enable GPU device access
within Docker containers. This can be found at
[NVIDIA/nvidia-docker](https://github.com/NVIDIA/nvidia-docker).


### Features

We have three types of docker files:

| Type  | Feature |
|-------------|---------|
| `dockerfile-runtime`   | The lightweight dockerfile which does not support CUDA compile |
| `dockerfile-devel` | The heavyweight dockerfile which supports cuda compile, use it if you need to compile packages with CUDA |
| `dockerfile-apex` | You will need that if you only want to build the packages once and need a lightweight docker image |


### Build images

1. Modify the `environment.yml` to add your own packages, and move it to the directory of docker file. 

2. Build the docker images using the following command:

```bash
$ docker pull muyeby/docker-pytorch:torch1.4
```

### Prebuilt images

Pre-built images are available on Docker Hub under the name
[muyeby/pytorch](https://hub.docker.com/r/muyeby/pytorch/). For example,
you can pull the CUDA 10.0 version Pytorch1.4 with:

```bash
$ docker pull muyeby/docker-pytorch:torch1.4
```

The table below lists software versions for each of the currently supported

| CUDA  | PyTorch |
|-------------|---------|
| `no-cuda`   | 1.2.0   |
| `cuda-11.3` | 1.10.0, 1.12.0  |
| `cuda-11.1` | 1.8.1   |
| `cuda-10.2` | 1.5.1, 1.6.0, 1.8.1 |
| `cuda-10.1` | 1.3.1, 1.4.0 |
| `cuda-10.0` | 1.0.1, 1.1.0, 1.2.0 |
| `cuda-9.2`  | 0.4.1   |
| `cuda-9.1`  | 0.4.0   |
| `cuda-9.0`  | 0.3.0, 0.3.1, 0.4.1 |
| `cuda-8.0`  | 0.2.0, 0.3.0, 0.3.1 |
| `cuda-7.5`  | 0.3.0   |