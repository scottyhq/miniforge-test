FROM ubuntu:18.04

ENV MINICONDA_VERSION=4.7.12.1
ENV CONDA_DIR=/home/ubuntu/miniconda3

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

# Install Miniconda
# Update versions to match latest miniforge
RUN URL="https://repo.anaconda.com/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh" \
    && INSTALLER="miniconda.sh" \
    && wget --quiet $URL -O ${HOME}/${INSTALLER}  \
    && /bin/bash ${HOME}/${INSTALLER} -b -p ${CONDA_DIR} \ 
#    && ${CONDA_DIR}/bin/conda update -n base -c defaults conda=4.8.2 python=3.7.6 \
    && ${CONDA_DIR}/bin/conda init bash \ 
    && rm ${HOME}/${INSTALLER}

# Create new environment with same python version as base
#RUN conda create --name myclone --clone base 
#RUN conda create -n test python=3.7.4=h265db76_1
