FROM nvidia/cuda:10.0-base-ubuntu16.04

# Config Software Versions here
ENV CONDA_VERSION=3-4.6.14
ENV PYTHON_VERSION=3.6.5
ENV PYTHON_NAME=py36
ENV PYTORCH_VERSION=1.0.0
ENV TORCHVISION_VERSION=0.2.1

# Install some basic utilities
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    vim \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
    make \
    gcc \
    g++ \
  && rm -rf /var/lib/apt/list/*

# Install Miniconda
RUN curl -so /opt/miniconda.sh https://repo.continuum.io/miniconda/Miniconda${CONDA_VERSION}-Linux-x86_64.sh \
  && chmod +x /opt/miniconda.sh \
  && /opt/miniconda.sh -b -p /opt/miniconda \
  && rm /opt/miniconda.sh
ENV PATH=/opt/miniconda/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Create a Python environment
RUN /opt/miniconda/bin/conda install conda-build \
  && /opt/miniconda/bin/conda create -y --name ${PYTHON_NAME} python=${PYTHON_VERSION} \
  && /opt/miniconda/bin/conda clean -ya
ENV CONDA_DEFAULT_ENV=${PYTHON_NAME}
ENV CONDA_PREFIX=/opt/miniconda/envs/${CONDA_DEFAULT_ENV}
ENV PATH=${CONDA_PREFIX}/bin:${PATH}

# Install Pytorch and Torchvision
# Note: for pytorch version..., use conda search to get its detailed versions.
RUN conda install -y -c pytorch \
    pytorch=${PYTORCH_VERSION} \
    torchvision=${TORCHVISION_VERSION} \
    cudatoolkit=10.0 \
  && conda clean -ya

# Install some basic python libraries for deep learning
RUN conda install -y pillow scikit-image -c anaconda \
  && conda clean -ya

# For other libs, users can install them based on this image
RUN conda install -y matplotlib tqdm \
  && conda clean -ya  