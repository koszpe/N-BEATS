FROM nvidia/cuda:10.2-cudnn7-devel

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-key del 7fa2af80
RUN rm /etc/apt/sources.list.d/nvidia-ml.list
RUN rm /etc/apt/sources.list.d/cuda.list
RUN apt-get update; apt-get install -y --no-install-recommends wget
RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-keyring_1.0-1_all.deb
RUN dpkg -i cuda-keyring_1.0-1_all.deb

# Install system packages
RUN apt-get update  -y --fix-missing && \
    apt-get install -y --no-install-recommends \
    software-properties-common \
    wget \
    curl \
    unrar \
    unzip \
    openssh-server \
    git && \
    apt-get clean -y

# Python
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    bash Miniconda3-latest-Linux-x86_64.sh -p /miniconda -b  && \
    rm -rf Miniconda3-latest-Linux-x86_64.sh

ENV PATH=/miniconda/bin:${PATH}
RUN conda update -y conda

RUN conda install -c anaconda -y \
      python=3.7.2 \
      pip

# JupyterLab
RUN conda install -c conda-forge jupyterlab

# Main frameworks
RUN pip install torch

# Install requirements
COPY requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

RUN echo "%docker  ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/inside_sudoers

COPY entry.sh /entry.sh
RUN chmod +x /entry.sh
ENTRYPOINT ["/entry.sh"]