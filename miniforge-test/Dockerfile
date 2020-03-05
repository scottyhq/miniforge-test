FROM ubuntu:18.04

ENV MINIFORGE_VERSION=4.8.2-1
ENV CONDA_DIR=/home/ubuntu/miniforge3

# Apt Packages
RUN apt-get update --fix-missing \
    && apt-get install wget -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV UID=1000
ENV USER=ubuntu
RUN groupadd --gid ${UID} ${USER}  \
    && useradd --create-home --gid ${UID} --no-log-init --shell /bin/bash --uid ${UID} ${USER} \
    && ln -s ${CONDA_DIR}/etc/profile.d/conda.sh /etc/profile.d/conda.sh

USER ubuntu
WORKDIR /home/ubuntu

# Install Miniforge
# https://github.com/ContinuumIO/docker-images/issues/140
RUN URL="https://github.com/conda-forge/miniforge/releases/download/${MINIFORGE_VERSION}/Miniforge3-${MINIFORGE_VERSION}-Linux-x86_64.sh" \
    && INSTALLER="miniforge.sh" \
    && wget --quiet $URL -O ${HOME}/${INSTALLER}  \
    && mkdir ${HOME}/.conda \
    && /bin/bash ${HOME}/${INSTALLER} -b -p ${CONDA_DIR} \
    && ${CONDA_DIR}/bin/conda init bash \
    && rm ${HOME}/${INSTALLER}

# Create new environment with same python version as base
#RUN conda create --name myclone --clone base
#RUN conda create -n test python=3.7.6=h357f687_2 -c conda-forge
